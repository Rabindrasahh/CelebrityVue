<template>
	<div class="">
		<section>
			<div class="container">
				<div class="row">
					<div class="col-xl-4 col-lg-5 col-md-7 mx-auto">
						<div class="card z-index-0 mt-sm-10 mt-7 mb-4">
							<div class="card-header text-center pt-4 pb-1">
								<h4 class="font-weight-bolder text-emerald mb-1">Login</h4>
								<p class="mb-0 text-bottle">Welcome Back!</p>
							</div>
							<div class="card-body">
								<form @submit.prevent="submitForm">
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

									<div class="text-center">
										<button
											type="submit"
											class="btn bg-gradient-main text-white w-100 my-4 mb-2"
										>Login</button>
									</div>
									<p class="text-sm text-center mt-3 mb-0">Don't remember your password? <router-link
											:to="{name:'resetPasswordRequest'}"
											class="text-bottle font-weight-bolder"
										>Reset password</router-link>
									</p>
									<p class="text-sm text-center mt-3 mb-0">Don't have an account? <router-link
											:to="{name:'signup'}"
											class="text-bottle font-weight-bolder"
										>Sign up</router-link>
									</p>
								</form>
							</div>
						</div>
					</div>
				</div>
			</div>
		</section>
		<loading
			:can-cancel="true"
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
		};
	},
	data() {
		return {
			email: "",
			password: "",
			prevRoute: null,
			isLoading: false,
		};
	},
	methods: {
		async submitForm() {
			const result = await this.v$.$validate();

			if (result) {
				this.isLoading = true;
				try {
					await this.$store.dispatch("auth/login", {
						password: this.password,
						email: this.email,
					});
					if (
						this.prevRoute.name !== "signup" &&
						this.prevRoute.name !== "resetPassword"
					)
						this.$router.replace(this.prevRoute.path);
					else this.$router.replace({ name: "index" });
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