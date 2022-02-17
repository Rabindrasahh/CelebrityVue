<template>
	<div>
		<the-navbar></the-navbar>
		<div class="container p-0 mt-4">
			<div class="card shadow-none">
				<div class="card-header">
					<h4>Receive a Marketing video message from <span class="text-emerald"> {{ talent.name }} </span> </h4>
				</div>
				<div class="card-body text-dark">
					<form
						@submit.prevent="submitForm"
						v-if="!showPaymentBox"
					>
						<div class="row">
							<div class="form-group col-md-6">
								<label for="">Company Name:</label>
								<input
									type="text"
									v-model="companyName"
									class="form-control"
									placeholder="Company Name"
									:class="{'is-invalid':v$.companyName.$error}"
								>
								<span
									v-if="v$.companyName.$error"
									class="text-sm text-danger"
								>
									{{ v$.companyName.$errors[0].$message }}
								</span>
							</div>

							<div class="form-group col-md-6">
								<label for="">Delivery Email:</label>
								<input
									type="text"
									class="form-control"
									placeholder="Email"
									v-model="email"
									:class="{'is-invalid':v$.email.$error}"
								>
								<span
									v-if="v$.email.$error"
									class="text-sm text-danger"
								>
									{{ v$.email.$errors[0].$message }}
								</span>
							</div>
							<div class="form-group">
								<label for="">Instructions:</label>
								<textarea
									class="form-control"
									:placeholder="`Give your instructions so that ${talent.name} can produce your video message in the best possible conditions.`"
									v-model="instructions"
									rows="5"
									:class="{'is-invalid':v$.instructions.$error}"
								></textarea>
								<span
									v-if="v$.instructions.$error"
									class="text-sm text-danger"
								>
									{{ v$.instructions.$errors[0].$message }}
								</span>
							</div>
						</div>
						<div class="text-center mt-3">
							<button class="btn bg-gradient-main text-white text-lg w-100 w-md-50">
								<i class="fa-duotone fa-credit-card me-2"></i>
								<span>ORDER FOR {{talent.marketingPrice}}$</span>
							</button>
						</div>
					</form>
					<!-- STRIPE ELEMENT -->
					<form
						@submit.prevent="doPayment"
						:class="{'d-none':!showPaymentBox}"
					>
						<div
							class="mt-3"
							id="payment-element"
						>
							<hr>
						</div>
						<div class="text-center mt-3">
							<button
								class="btn bg-gradient-dark mt-3 text-lg w-100 w-md-50"
								type="submit"
							>
								<i class="fa-duotone fa-credit-card me-2"></i>
								<span> PAY </span>
							</button>
						</div>
					</form>
					<!-- END STRIPE ELEMENT -->
				</div>
			</div>
		</div>
		<loading
			:can-cancel="false"
			v-model:active="isLoading"
		/>
	</div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required, helpers, email } from "@vuelidate/validators";
export default {
	setup: () => ({ v$: useVuelidate() }),
	validations() {
		return {
			companyName: {
				required: helpers.withMessage(
					"Please Enter Your Company Name",
					required
				),
			},
			instructions: {
				required: helpers.withMessage(
					"Please Enter Your Instructions",
					required
				),
			},
			email: {
				required: helpers.withMessage(
					"Please Enter Your Email",
					required
				),
				email: helpers.withMessage(
					"Please Enter Your Email Correctly",
					email
				),
			},
		};
	},
	props: ["talentId"],
	data() {
		return {
			companyName: "",
			email: "",
			instructions: "",
			isLoading: false,
			showPaymentBox: false,
			stripe: {},
			elements: {},
		};
	},
	methods: {
		async init() {
			this.isLoading = true;
			try {
				await this.$store.dispatch("talent/getTalent", {
					talentId: this.talentId,
				});
			} catch (err) {
				this.$swal({
					icon: "error",
					text: err.message,
				});
			}
			this.isLoading = false;
		},
		async submitForm() {
			const result = await this.v$.$validate();
			if (!result) return;

			this.isLoading = true;
			try {
				// get payment intent
				await this.$store.dispatch("site/getPaymentIntent", {
					talentId: this.talentId,
					type: "normal",
				});
				const clientSecret =
					this.$store.getters["site/paymentIntent"]?.clientSecret;
				this.showPaymentBox = true;

				// STRIPE INIT

				const appearance = {
					theme: "stripe",
				};
				this.elements = this.stripe.elements({
					appearance,
					clientSecret,
				});
				const paymentElement = this.elements.create("payment");
				paymentElement.mount("#payment-element");

				localStorage.setItem(
					"o",
					JSON.stringify({
						type: "Marketing",
						companyName: this.companyName,
						instructions: this.instructions,
						email: this.email,
						talentId: this.talentId,
					})
				);
			} catch (err) {
				this.$swal({
					icon: "error",
					text: err.message,
				});
			}
			this.isLoading = false;
		},
		async doPayment() {
			this.isLoading = true;

			const { error } = await this.stripe.confirmPayment({
				elements: this.elements,
				confirmParams: {
					// Make sure to change this to your payment completion page

					return_url: `https://dzicace.netlify.app${window.location.pathname}`,
				},
			});
			if (
				error.type === "card_error" ||
				error.type === "validation_error"
			) {
				this.$swal({
					icon: "error",
					text: error.messsage,
				});
			}
		},
		async verifyPayment() {
			this.isLoading = true;
			const clientSecret = this.$route.query.payment_intent_client_secret;
			if (!clientSecret) return;

			const { paymentIntent } = await this.stripe.retrievePaymentIntent(
				clientSecret
			);
			const paymentId = paymentIntent.id;
			const status = paymentIntent.status;

			if (status === "succeeded") {
				let data = localStorage.getItem("o");
				data = JSON.parse(data);
				await this.$store.dispatch("site/bookVideo", {
					...data,
					paymentId,
				});

				this.$swal({
					icon: "success",
					text: "Your purchase was successfull!",
				});
				localStorage.removeItem("o");
				this.$router.replace({ name: "userPanelPendingOrders" });
			} else if (status === "processing") {
				this.$swal({
					icon: "warning",
					text: "Your purchase is processing",
				});
			} else if (status === "requires_payment_method") {
				this.$swal({
					icon: "error",
					text: "Your payment was not successful, please try again.",
				});
			} else {
				this.$swal({
					icon: "error",
					text: "Something went wrong.",
				});
			}
		},
	},
	computed: {
		talent() {
			return this.$store.getters["talent/talent"]?.talent;
		},
	},
	created() {
		this.stripe = global.Stripe(
			"pk_test_51KJ0PwABlJOAQIO6iCsxrVlLO5nasgOGUAJIHxo5ZpM6Z0vu713Lm8hRNNBzUdS5i04F40tlx39rLyjirL1Yx4KH0068wqg2vO"
		);

		// if payment completed, this block will run
		const paymentIntentQuery =
			this.$route.query.payment_intent_client_secret;
		if (paymentIntentQuery) this.verifyPayment();

		this.init();
	},
};
</script>