<template>
  <div>
    <the-navbar></the-navbar>
    <div class="container p-0 mt-4">
      <div class="card shadow-none">
        <div class="card-header">
          <h4>
            Receive a personalized video message from
            <span class="text-emerald"> {{ talent.name }} </span>
          </h4>
        </div>
        <div class="card-body text-dark">
          <form v-if="!showPaymentBox" @submit.prevent="submitForm">
            <p class="fw-bold">Who is the video message intended for?</p>
            <div class="form-check">
              <label for="ownerMe">Me</label>
              <input
                type="radio"
                class="form-check-input"
                name="owner"
                id="ownerMe"
                v-model="videoOwner"
                value="Me"
              />
            </div>
            <div class="form-check">
              <label for="ownerOthers">Someone else</label>
              <input
                type="radio"
                class="form-check-input"
                name="owner"
                id="ownerOthers"
                v-model="videoOwner"
                value="Someone"
              />
            </div>
            <hr />
            <div v-if="videoOwner === 'Me'">
              <div class="row">
                <div class="form-group col-md-6">
                  <label for="">Your First Name:</label>
                  <input
                    type="text"
                    v-model="name"
                    class="form-control"
                    placeholder="Name"
                    :class="{ 'is-invalid': v$.name.$error }"
                  />
                  <span v-if="v$.name.$error" class="text-sm text-danger">
                    {{ v$.name.$errors[0].$message }}
                  </span>
                </div>
                <div class="form-group col-md-6">
                  <label for="">For which occasion?</label>
                  <select
                    v-model="occasion"
                    class="form-control"
                    placeholder="Name"
                    :class="{ 'is-invalid': v$.occasion.$error }"
                  >
                    <option value="" disabled="">Choose an occasion</option>
                    <option value="anniversary">🎁 Birthday</option>
                    <option value="other">🔎 Other</option>
                    <option value="advice">🧐 Tips</option>
                    <option value="diploma">🎓 Diploma</option>
                    <option value="encouragements">💪 Encouragement</option>
                    <option value="congratulations">🎉 Congratulations</option>
                    <option value="weeding">💍 Wedding</option>
                    <option value="birth">👶 Birth</option>
                    <option value="questions">❓ Questions</option>
                  </select>
                </div>
                <div class="form-group">
                  <label for="">Delivery Email:</label>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Email"
                    v-model="email"
                    :class="{ 'is-invalid': v$.email.$error }"
                  />
                  <span v-if="v$.email.$error" class="text-sm text-danger">
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
                    :class="{ 'is-invalid': v$.instructions.$error }"
                  ></textarea>
                  <span
                    v-if="v$.instructions.$error"
                    class="text-sm text-danger"
                  >
                    {{ v$.instructions.$errors[0].$message }}
                  </span>
                </div>
                <div class="form-check ms-2">
                  <label for="visibleOnProfile"
                    >Make this video visible on {{ talent.name }} profile
                  </label>
                  <input
                    type="checkbox"
                    v-model="visibleOnProfile"
                    class="form-check-input"
                    id="visibleOnProfile"
                  />
                </div>
                <!-- <hr class="mt-2">
								<p class="fw-bold">Delivery options:</p>
								<div class="form-check ms-2">
									<label for="">Make this video visible on Coumba Baradji's profile </label>
									<input
										type="checkbox"
										v-model="visibleOnProfile"
										class="form-check-input"
									>
								</div> -->
              </div>
            </div>
            <div v-else>
              <div class="row">
                <div class="form-group col-md-4">
                  <label for="">From:</label>
                  <input
                    type="text"
                    v-model="from"
                    class="form-control"
                    placeholder="Alex"
                    :class="{ 'is-invalid': v$.from.$error }"
                  />
                  <span v-if="v$.from.$error" class="text-sm text-danger">
                    {{ v$.from.$errors[0].$message }}
                  </span>
                </div>
                <div class="form-group col-md-4">
                  <label for="">To:</label>
                  <input
                    type="text"
                    v-model="to"
                    class="form-control"
                    placeholder="Marie"
                    :class="{ 'is-invalid': v$.to.$error }"
                  />
                  <span v-if="v$.to.$error" class="text-sm text-danger">
                    {{ v$.to.$errors[0].$message }}
                  </span>
                </div>
                <div class="form-group col-md-4">
                  <label for="">For which occasion?</label>
                  <select
                    v-model="occasion"
                    class="form-control"
                    placeholder="Name"
                    :class="{ 'is-invalid': v$.occasion.$error }"
                  >
                    <option value="" disabled="">Choose an ountry</option>
                    <option value="anniversary">🎁 Birthday</option>
                    <option value="other">🔎 Other</option>
                    <option value="advice">🧐 Tips</option>
                    <option value="diploma">🎓 Diploma</option>
                    <option value="encouragements">💪 Encouragement</option>
                    <option value="congratulations">🎉 Congratulations</option>
                    <option value="weeding">💍 Wedding</option>
                    <option value="birth">👶 Birth</option>
                    <option value="questions">❓ Questions</option>
                  </select>
                </div>
                <div class="form-group">
                  <label for="">Delivery Email:</label>
                  <input
                    type="text"
                    class="form-control"
                    placeholder="Email"
                    v-model="email"
                    :class="{ 'is-invalid': v$.email.$error }"
                  />
                </div>
                <div class="form-group">
                  <label for="">Instructions:</label>
                  <textarea
                    class="form-control"
                    :placeholder="`Give your instructions so that ${talent.name} can produce your video message in the best possible conditions.`"
                    v-model="instructions"
                    :class="{ 'is-invalid': v$.instructions.$error }"
                    rows="5"
                  ></textarea>
                  <span
                    v-if="v$.instructions.$error"
                    class="text-sm text-danger"
                  >
                    {{ v$.instructions.$errors[0].$message }}
                  </span>
                </div>
                <div class="form-check ms-2">
                  <label for="visibleOnProfile"
                    >Make this video visible on {{ talent.name }} profile
                  </label>
                  <input
                    type="checkbox"
                    v-model="visibleOnProfile"
                    class="form-check-input"
                    id="visibleOnProfile"
                  />
                </div>
                <!-- <hr class="mt-2">
								<p class="fw-bold">Delivery options:</p>
								<div class="form-check ms-2">
									<label for="">Make this video visible on Coumba Baradji's profile </label>
									<input
										type="checkbox"
										v-model="visibleOnProfile"
										class="form-check-input"
									>
								</div> -->
              </div>
            </div>
            <div class="text-center mt-3">
              <button
                class="btn bg-gradient-main text-white text-lg w-100 w-md-50"
              >
                <i class="fa-duotone fa-credit-card me-2"></i>
                <span>ORDER FOR {{ talent.normalPrice }}$</span>
              </button>
            </div>
          </form>
          <!-- STRIPE ELEMENT -->
          <form
            @submit.prevent="doPayment"
            :class="{ 'd-none': !showPaymentBox }"
          >
            <div class="mt-3" id="payment-element">
              <hr />
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
    <loading :can-cancel="false" v-model:active="isLoading" />
  </div>
</template>

<script>
const someoneVerify = function (value) {
  if (!value && this.videoOwner === "Someone") return false;
  else return true;
};
const meVerify = function (value) {
  if (!value && this.videoOwner === "Me") return false;
  else return true;
};

import useVuelidate from "@vuelidate/core";
import { required, helpers, email } from "@vuelidate/validators";
export default {
  setup: () => ({ v$: useVuelidate() }),
  validations() {
    return {
      name: {
        meVerify: helpers.withMessage("Please Enter Your Name", meVerify),
      },
      to: {
        someoneVerify: helpers.withMessage(
          "Please Enter To field",
          someoneVerify
        ),
      },
      from: {
        someoneVerify: helpers.withMessage(
          "Please Enter From Field",
          someoneVerify
        ),
      },
      occasion: {
        required: helpers.withMessage("Please Enter the Occasion", required),
      },
      instructions: {
        required: helpers.withMessage(
          "Please Enter Your Instructions",
          required
        ),
      },
      email: {
        required: helpers.withMessage("Please Enter Your Email", required),
        email: helpers.withMessage("Please Enter Your Email Correctly", email),
      },
    };
  },
  props: ["talentId"],
  data() {
    return {
      videoOwner: "Me",
      from: "",
      to: "",
      name: "",
      occasion: "",
      email: "",
      instructions: "",
      visibleOnProfile: false,
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

        // set payment data in localstorage
        localStorage.setItem(
          "o",
          JSON.stringify({
            type: "Normal",
            videoOwner: this.videoOwner,
            from: this.from,
            to: this.to,
            name: this.name,
            occasion: this.occasion,
            instructions: this.instructions,
            email: this.email,
            isPublicOnProfile: this.visibleOnProfile,
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
      if (error.type === "card_error" || error.type === "validation_error") {
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
      const status = paymentIntent.status;
      const paymentId = paymentIntent.id;

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
    const paymentIntentQuery = this.$route.query.payment_intent_client_secret;
    if (paymentIntentQuery) this.verifyPayment();

    this.init();
  },
};
</script>