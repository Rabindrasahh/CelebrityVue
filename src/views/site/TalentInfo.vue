<template>
  <div>
    <the-navbar></the-navbar>
    <div class="container mt-4">
      <div class="col-lg-12">
        <section class="py-4 position-relative overflow-hidden">
          <img
            src="../../assets/img/shapes/waves-white.svg"
            alt="pattern-lines"
            class="position-absolute top-0 w-100"
          />
          <div class="container-fluid">
            <div class="row">
              <div class="col-12 mx-auto bg-gradient-main border-radius-lg">
                <div class="row py-5">
                  <div
                    class="
                      col-xl-4 col-md-6
                      px-5
                      position-relative
                      mb-3
                      text-center
                    "
                  >
                    <video
                      controls
                      autoplay="autoplay"
                      class="w-100 h-100 border-raduis-lg"
                      :key="`https://dzicace.herokuapp.com/${talent?.introVideo}`"
                      v-if="talent?.introVideo"
                      style="object-fit: cover"
                    >
                      <source
                        :src="`https://dzicace.herokuapp.com/${talent?.introVideo}`"
                        :type="`video/${talent?.introVideo?.split('.')[1]}`"
                      />
                      Sorry, your browser doesn't support embedded videos.
                    </video>
                    <img
                      class="
                        img
                        border-radius-md
                        max-width-300
                        h-100
                        w-100
                        position-relative
                        z-index-2
                      "
                      v-else-if="talent.profileImage"
                      :src="`https://dzicace.herokuapp.com/${talent.profileImage}`"
                    />
                    <img
                      class="
                        img
                        border-radius-md
                        max-width-300
                        h-100
                        w-100
                        position-relative
                        z-index-2
                      "
                      v-else
                      src="../../assets/img/user-placeholder.png"
                    />
                  </div>
                  <div
                    class="
                      col-xl-4 col-md-5
                      z-index-2
                      position-relative
                      px-md-3
                      my-auto
                      px-4
                    "
                  >
                    <div
                      class="
                        d-flex
                        mb-2
                        align-items-center
                        justify-content-center justify-content-md-start
                      "
                    >
                      <!-- <img
												src="../../assets/img/team-3.jpg"
												class="rounded-circle img"
												alt=""
											> -->
                      <h3 class="text-white mb-0">{{ talent.name }}</h3>
                    </div>
                    <p class="text-white text-sm text-center text-md-start">
                      <span class="me-2"> {{ talent.categoryId?.name }} </span>
                      <i
                        class="fa-solid fa-star text-warning"
                        v-for="i in talent.rating"
                        :key="i"
                      ></i>
                      <i
                        class="fa-regular fa-star text-warning"
                        v-for="i in 5 - talent.rating"
                        :key="i"
                      ></i>
                    </p>

                    <p class="text-lg text-white text-center text-md-start">
                      {{ talent.description }}
                    </p>
                  </div>
                  <div class="col-1"></div>
                  <div
                    class="
                      col-xl-2 col-12
                      px-xl-0 px-5
                      my-xl-auto
                      position-relative
                      z-index-1
                      text-center
                    "
                  >
                    <div class="d-none d-xl-block">
                      <div class="d-flex flex-wrap justify-content-center">
                        <i
                          @click="copy"
                          class="
                            fa-regular fa-share-nodes
                            text-white text-lg
                            cursor-pointer
                          "
                        ></i>
                        <i
                          @click="bookmark"
                          class="
                            fa-bookmark
                            text-white text-lg
                            ms-3
                            cursor-pointer
                          "
                          :class="{
                            fas: isBookmarked,
                            'fa-regular': !isBookmarked,
                          }"
                        ></i>
                      </div>
                      <hr class="text-white" />
                    </div>
                    <h3
                      class="text-white mt-xl-0 mt-5"
                      v-if="talent.requestsCount > 10"
                    >
                      +{{ (Math.ceil(talent.requestsCount / 10) - 1) * 10 }}
                    </h3>
                    <h3 class="text-white mt-xl-0 mt-5" v-else>&lt;10</h3>
                    <p class="text-sm text-white">
                      Users booked video from {{ talent.name }}
                    </p>
                    <router-link
                      :to="{
                        name: 'bookNormalVideo',
                        params: { talentId: talent._id },
                      }"
                      v-if="isAuth"
                      class="btn btn-outline-white icon-move-right text-sm"
                      >Book Now for ${{ talent.normalPrice }} (Normal)
                      <i class="fas fa-arrow-right text-xs"></i>
                    </router-link>
                    <router-link
                      :to="{ name: 'login' }"
                      v-else
                      class="btn btn-outline-white icon-move-right text-sm"
                      >Book Now for ${{ talent.normalPrice }} (Normal)
                      <i class="fas fa-arrow-right text-xs"></i>
                    </router-link>

                    <router-link
                      :to="{
                        name: 'bookMarketingVideo',
                        params: { talentId: talent._id },
                      }"
                      v-if="isAuth"
                      class="btn btn-outline-white icon-move-right text-sm"
                      >Book Now for ${{ talent.marketingPrice }} (Marketing)
                      <i class="fas fa-arrow-right text-xs"></i>
                    </router-link>
                    <router-link
                      :to="{ name: 'login' }"
                      v-else
                      class="btn btn-outline-white icon-move-right text-sm"
                      >Book Now for ${{ talent.marketingPrice }} (Marketing)
                      <i class="fas fa-arrow-right text-xs"></i>
                    </router-link>
                    <div class="d-block d-xl-none">
                      <hr class="text-white" />
                      <div class="d-flex flex-wrap justify-content-center">
                        <i
                          class="
                            fa-regular fa-share-nodes
                            text-white text-lg
                            cursor-pointer
                          "
                        ></i>
                        <i
                          @click="bookmark"
                          class="
                            fa-bookmark
                            text-white text-lg
                            ms-3
                            cursor-pointer
                          "
                          :class="{
                            fas: isBookmarked,
                            'fa-regular': !isBookmarked,
                          }"
                        ></i>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
      <!-- TODO: ADD VIDEOS -->
      <div class="row p-2">
        <div class="col-lg-8 ps-3 mt-3">
          <div class="card shadow-none bg-green">
            <div class="card-header bg-green pb-0">
              <h5>Comments</h5>
            </div>
            <div class="card-body">
              <ul>
                <li
                  class="comment"
                  v-for="(comment, i) in comments"
                  :key="comment"
                >
                  <div class="d-flex align-items-center">
                    <div class="comment__image">
                      {{
                        comment.userId?.fullName
                          ? comment.userId?.fullName.slice(0, 1)
                          : "U"
                      }}
                    </div>
                    <div class="ms-3 d-flex flex-column">
                      <span class="text-dark fw-bold">
                        {{
                          comment.userId?.fullName
                            ? comment.userId?.fullName
                            : "User"
                        }}
                      </span>
                      <div>
                        {{ comment.rating }}
                        <i
                          class="fa-solid fa-star text-bottle"
                          v-for="i in comment.comment?.rating"
                          :key="i"
                        ></i>
                        <i
                          class="fa-regular fa-star text-bottle"
                          v-for="i in 5 - comment.comment?.rating"
                          :key="i"
                        ></i>
                      </div>
                    </div>
                  </div>
                  <p class="ms-2 mt-3 fw-bold text-dark">
                    {{ comment.comment?.content }}
                  </p>
                  <hr v-if="i + 1 !== comments.length" />
                </li>
              </ul>
            </div>
          </div>
        </div>
        <div class="col-lg-4 mt-3">
          <div class="card shadow-none bg-green">
            <div class="card-header bg-green pb-0">
              <h5>How to book?</h5>
            </div>
            <div class="card-body">
              <div class="ps-lg-0 ps-0">
                <div class="p-3 info-horizontal">
                  <div
                    class="
                      icon icon-shape
                      rounded-circle
                      bg-gradient-main
                      shadow
                      text-center
                    "
                  >
                    <i class="fas fa-credit-card opacity-10"></i>
                  </div>
                  <div class="description ps-3 text-dark">
                    <p class="mb-0">
                      It becomes harder for us to give others a hand. <br />
                      We get our heart broken by people we love.
                    </p>
                  </div>
                </div>

                <div class="p-3 info-horizontal">
                  <div
                    class="
                      icon icon-shape
                      rounded-circle
                      bg-gradient-main
                      shadow
                      text-center
                    "
                  >
                    <i class="fas fa-clock opacity-10"></i>
                  </div>
                  <div class="description ps-3 text-dark">
                    <p class="mb-0">
                      As we live, our hearts turn colder. <br />Cause pain is
                      what we go through as we become older.
                    </p>
                  </div>
                </div>
                <div class="p-3 info-horizontal">
                  <div
                    class="
                      icon icon-shape
                      rounded-circle
                      bg-gradient-main
                      shadow
                      text-center
                    "
                  >
                    <i class="fas fa-video opacity-10"></i>
                  </div>
                  <div class="description ps-3 text-dark">
                    <p class="mb-0">
                      When we lose family over time. <br />
                      What else could rust the heart more over time? Blackgold.
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <loading
        v-model:active="isLoading"
        :can-cancel="false"
        :is-full-page="true"
      />
    </div>
    <the-footer></the-footer>
  </div>
</template>

<script>
export default {
  props: ["talentId"],
  data() {
    return {
      isLoading: false,
    };
  },
  methods: {
    async init() {
      this.isLoading = true;
      try {
        await this.$store.dispatch("talent/getTalent", {
          talentId: this.talentId,
        });
        if (this.isAuth) {
          await this.$store.dispatch("site/getIsBookmarked", {
            talentId: this.talentId,
          });
        }
      } catch (err) {
        this.$swal({
          icon: "error",
          text: err.message,
        });
      }
      this.isLoading = false;
    },
    async bookmark() {
      try {
        if (this.isBookmarked) {
          await this.$store.dispatch("site/unbookmark", {
            talentId: this.talentId,
          });
          this.$store.commit("site/setIsBookmarked", false);
        } else {
          await this.$store.dispatch("site/bookmark", {
            talentId: this.talentId,
          });
          this.$store.commit("site/setIsBookmarked", true);
        }
      } catch (err) {
        this.$swal({
          icon: "error",
          text: err.message,
        });
      }
    },
    copy() {
      navigator.clipboard.writeText(`Dzicace.com${this.$route.fullPath}`);
      this.$swal({
        icon: "success",
        text: "The link was copied successfully",
        toast: true,
        position: "top-end",
      });
    },
  },
  computed: {
    isAuth() {
      return this.$store.getters["auth/isAuthenticated"];
    },
    talent() {
      return this.$store.getters["talent/talent"]?.talent;
    },
    isBookmarked() {
      return this.$store.getters["site/isBookmarked"];
    },
    comments() {
      return this.$store.getters["talent/talent"].comments;
    },
  },
  created() {
    this.init();
  },
};
</script>

<style lang="scss" scoped>
@import "../../assets/scss/_variables.scss";
.img {
  width: 40px;
  object-fit: cover;
}
.bg-green {
  background-color: $color-turquoise-green-1;
}
ul {
  list-style: none;
  padding-left: 0px;
}
.comment {
  margin-bottom: 6px;
  &__image {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background-color: $color-bottle-green;
    color: white;
    font-size: 34px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
</style>