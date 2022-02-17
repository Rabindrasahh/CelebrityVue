<template>
  <div>
    <header class="header-rounded-images vh-100">
      <index-navbar></index-navbar>
      <div class="page-header min-vh-100">
        <div class="container">
          <div class="row position-relative" style="bottom: 25px">
            <div class="col-lg-6 text-lg-start text-center py-sm-0 py-7">
              <h1 class="text-white">
                Personalized videos from your favorite stars
              </h1>
              <p class="text-white text-lg-start text-center pe-5 mt-4">
                Access thousands of celebrities and request a personalized video
                message for any occasion.
              </p>
              <br />
              <div class="buttons">
                <button
                  type="button"
                  @click="explore"
                  class="
                    btn btn-lg
                    bg-gradient-main
                    text-white
                    btn-rounded
                    me-2
                  "
                >
                  Explore
                </button>
                <router-link
                  :to="{ name: 'talentsJoin' }"
                  type="button"
                  class="btn btn-lg btn-outline-white btn-rounded ms-xl-3"
                  >Join as celebrity</router-link
                >
              </div>
              <!-- <div class="row mt-5 justify-content-start">
							<div class="col-md-3 col-6 p-0">
								<img
									class="w-100"
									src="../../assets/img/logos/gray-logos/logo-pinterest.svg"
									alt="logo"
								>
							</div>
							<div class="col-md-3 col-6 p-0">
								<img
									class="w-100"
									src="../../assets/img/logos/gray-logos/logo-netflix.svg"
									alt="logo"
								>
							</div>
							<div class="col-md-3 col-6 p-0">
								<img
									class="w-100"
									src="../../assets/img/logos/gray-logos/logo-coinbase.svg"
									alt="logo"
								>
							</div>
							<div class="col-md-3 col-6 p-0">
								<img
									class="w-100"
									src="../../assets/img/logos/gray-logos/logo-nasa.svg"
									alt="logo"
								>
							</div>
						</div> -->
            </div>
            <div class="col-lg-6 text-end position-relative">
              <div class="position-absolute top-0 w-100">
                <img
                  src="../../assets/img/team-2.jpg"
                  class="
                    position-absolute
                    rounded-circle
                    img-1
                    d-lg-block d-none
                    z-index-3
                  "
                  alt="avatar"
                />
                <img
                  src="../../assets/img/shapes/shape-3.svg"
                  class="
                    top-0
                    end-0
                    position-absolute
                    shape-3
                    d-lg-block d-none
                  "
                  alt="shape"
                />
                <img
                  src="../../assets/img/shapes/shape-2.svg"
                  class="mt-4 position-absolute shape-2 d-lg-block d-none"
                  alt="shape"
                />
                <!-- <img
									src="../../assets/img/shapes/shape-1.svg"
									class="mt-n6 position-absolute shape-1 d-lg-block d-none"
									alt="shape"
								> -->
                <img
                  src="../../assets/img/marie.jpg"
                  class="
                    position-absolute
                    rounded-circle
                    img-2
                    d-lg-block d-none
                    z-index-3
                  "
                  alt="avatar"
                />
              </div>
            </div>
          </div>
        </div>
        <div
          class="
            min-vh-100
            w-100
            position-absolute
            mt-sm-n11
            top-0
            d-lg-none d-block
          "
          style="background: #101010"
        ></div>

        <div
          class="
            min-vh-100
            w-75
            position-absolute
            border-radius-section
            ms-n10
            mt-n10
            top-0
            d-lg-block d-none
          "
          style="background: #101010"
        ></div>
        <div
          class="
            min-vh-75
            w-50
            blur-section
            bg-gradient-main
            position-absolute
            border-top-start-radius-0
            border-top-end-radius-0
            border-bottom-end-radius-0
            border-radius-section
            mt-n8
            me-n4
            top-0
            end-0
            d-lg-block d-none
          "
        ></div>
      </div>
    </header>
    <!-- NEW CELEBRITIES -->
    <div class="container new">
      <div class="d-flex flex-wrap">
        <h2 class="text-uppercase text-dark">New Celebrities</h2>
        <router-link
          :to="{ name: 'allTalents' }"
          class="btn btn-outline-emerald ms-auto"
          >View More &rarr;</router-link
        >
      </div>
      <div class="row mt-3">
        <celebrity-card
          v-for="talent in talents"
          :key="talent._id"
          :data="talent"
        ></celebrity-card>
      </div>
    </div>
    <!-- CATEGORIES -->
    <div class="container mt-5">
      <div class="d-flex flex-wrap">
        <h2 class="text-uppercase text-dark">Categories</h2>
        <router-link
          :to="{ name: 'allCategories' }"
          class="btn btn-outline-emerald ms-auto"
          >View More &rarr;</router-link
        >
      </div>
      <div class="row">
        <router-link
          v-for="category in categories"
          :key="category._id"
          class="w-100 w-sm-50 w-md-30 w-xl-25 mt-3"
          :to="{ name: 'allTalents', query: { category: category.name } }"
        >
          <!-- linear-gradient(310deg, #52b788de, #40916de8), -->
          <div
            class="card category category__tv text-center"
            :style="{
              'background-image': `${
                category.gradient
              }, url(https://dzicace.herokuapp.com/${category.image?.replaceAll()})`,
            }"
          >
            <h3 class="text-white my-auto">{{ category.name }}</h3>
          </div>
        </router-link>
      </div>
    </div>
    <!-- ARTICLES -->
    <div class="container mt-5">
      <div class="d-flex flex-wrap">
        <h2 class="text-uppercase text-dark">Articles</h2>
        <router-link
          :to="{ name: 'articles' }"
          class="btn btn-outline-emerald ms-auto"
          >View More &rarr;</router-link
        >
      </div>
      <div class="row justify-content-around">
        <article-card
          v-for="article in articles"
          :key="article"
          :data="article"
        ></article-card>
      </div>
    </div>
    <loading
      v-model:active="isLoading"
      :can-cancel="false"
      :is-full-page="true"
    />

    <the-footer></the-footer>
  </div>
</template>

<script>
import IndexNavbar from "../../components/layouts/IndexNavbar.vue";
export default {
  components: { IndexNavbar },
  data() {
    return {
      isLoading: false,
    };
  },
  methods: {
    explore() {
      document.querySelector(".new").scrollIntoView();
    },
    async init() {
      this.isLoading = true;
      try {
        await this.$store.dispatch("site/getIndexData");
      } catch (err) {
        this.$swal({
          icon: "error",
          text: err.message,
        });
      }
      this.isLoading = false;
    },
    getRandomGradient() {
      const gradients = [
        `linear-gradient(310deg, #ffafbd , #ffc3a0)`,
        `linear-gradient(310deg, #2193b0  , #6dd5ed)`,
        `linear-gradient(310deg, #de6262  , #ffb88c)`,
        `linear-gradient(310deg, #06beb6  , #06beb6 )`,
        `linear-gradient(310deg, #eb3349   , #f45c43 )`,
        `linear-gradient(310deg, #56ab2f    , #a8e063 )`,
        `linear-gradient(310deg, #614385    , #516395 )`,
        `linear-gradient(310deg, #02aab0    , #00cdac )`,
        `linear-gradient(310deg, #ff512f    , #dd2476 )`,
        `linear-gradient(310deg, #4568dc     , #b06ab3 )`,
      ];
      return gradients[Math.floor(Math.random() * gradients.length)];
    },
  },
  computed: {
    talents() {
      return this.$store.getters["site/indexData"].newTalents;
    },
    categories() {
      return this.$store.getters["site/indexData"].categories?.map((c) => {
        return {
          ...c,
          image: c.image?.replaceAll(
            String.fromCharCode(92),
            String.fromCharCode(47)
          ),
          gradient: this.getRandomGradient(),
        };
      });
    },
    articles() {
      return this.$store.getters["site/indexData"].articles;
    },
  },
  created() {
    this.init();
  },
};
</script>

<style scoped lang="scss">
.nav {
  padding: 20px 15px;
  &__image {
    width: 100px;
  }
}

.nav-link {
  font-size: 19px;
}

.new {
  margin-top: 0;

  @media screen and (max-width: 992px) {
    margin-top: -90px;
  }

  @media screen and (max-width: 576px) {
    margin-top: 60px;
  }
}

.navbar-togglerr {
  border: none;
  outline: none;
  background-color: transparent;
  font-size: 30px;
  color: white;

  @media screen and (min-width: 992px) {
    display: none;
  }
}

.category {
  height: 180px;
  background-position: center;
}
</style>