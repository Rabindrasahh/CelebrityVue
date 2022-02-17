<template>
	<div class="">
		<section>
			<div class="container">
				<div class="row">
					<div class="col-xl-4 col-lg-5 col-md-7 mx-auto">
						<div class="card z-index-0 mt-sm-10 mt-7 mb-4">
							<div class="card-header text-center pt-4 pb-1">
								<h4 class="font-weight-bolder text-emerald mb-1">Sign Up</h4>
								<p class="mb-0 text-bottle">Book personalized video shoutouts from your favorite people</p>
							</div>
							<div class="card-body">
								<form @submit.prevent="submitForm">
									<div class="mb-3">
										<input
											type="text"
											class="form-control"
											:class="{'is-invalid':v$.fullName.$error}"
											placeholder="Full Name"
											v-model="fullName"
										/>
										<span
											v-if="v$.fullName.$error"
											class="text-sm text-danger"
										>
											{{ v$.fullName.$errors[0].$message }}
										</span>
									</div>
									<div class="mb-3">
										<input
											type="email"
											class="form-control"
											:class="{'is-invalid':v$.email.$error}"
											placeholder="Email"
											v-model="email"
										/>
										<span
											v-if="v$.email.$error"
											class="text-sm text-danger"
										>
											{{ v$.email.$errors[0].$message }}
										</span>
									</div>
									<div class="mb-3">
										<input
											type="password"
											class="form-control"
											:class="{'is-invalid':v$.password.$error}"
											placeholder="Password"
											v-model="password"
										/>
										<span
											v-if="v$.password.$error"
											class="text-sm text-danger"
										>
											{{ v$.password.$errors[0].$message }}
										</span>
									</div>
									<div class="form-check form-check-info text-left">
										<input
											class="form-check-input"
											type="checkbox"
											id="flexCheckDefault"
											v-model="isAgree"
										>
										<label
											class="form-check-label"
											for="flexCheckDefault"
										>
											I agree the <a
												href="#"
												class="text-dark font-weight-bolder"
											>Terms and Conditions</a>
										</label>
										<span
											v-if="v$.isAgree.$error"
											class="text-sm text-danger d-block"
										>
											{{ v$.isAgree.$errors[0].$message }}
										</span>
									</div>
									<div class="text-center">
										<button
											type="submit"
											class="btn bg-gradient-main text-white w-100 my-4 mb-2"
										>Sign up</button>
									</div>
									<p class="text-sm text-center mt-3 mb-0">Already have an account? <router-link
											:to="{name:'login'}"
											class="text-bottle font-weight-bolder"
										>Sign in</router-link>
									</p>
								</form>
							</div>
						</div>
					</div>
				</div>
			</div>
			<loading
				:can-cancel="true"
				v-model:active="isLoading"
			/>
		</section>

	</div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required, helpers, email } from "@vuelidate/validators";

const isTrue = (value) => value;
export default {
	setup: () => ({ v$: useVuelidate() }),
	validations() {
		return {
			fullName: {
				required: helpers.withMessage(
					"Please Enter Your Full Name",
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
			password: {
				required: helpers.withMessage(
					"Please Enter Your Password",
					required
				),
			},
			isAgree: {
				isTrue: helpers.withMessage(
					"Please Accept The Agreements",
					isTrue
				),
			},
		};
	},
	data() {
		return {
			isLoading: false,
			fullName: "",
			email: "",
			password: "",
			isAgree: false,
			prevRoute: null,
		};
	},
	methods: {
		async submitForm() {
			const result = await this.v$.$validate();

			if (result) {
				this.isLoading = true;
				try {
					await this.$store.dispatch("auth/signup", {
						fullName: this.fullName,
						password: this.password,
						email: this.email,
						setToken: true,
					});
					this.$router.replace(this.prevRoute.path);
				} catch (err) {
					this.$swal({
						icon: "error",
						text: err.message,
					});
				}
				this.isLoading = false;
			}
		},
	},
	created() {
		document.body.style.backgroundColor = "#e9ecef";
	},
	beforeRouteEnter(to, from, next) {
		next((vm) => {
			vm.prevRoute = from;
		});
	},
};
</script>

<style lang="scss" scoped>
</style>