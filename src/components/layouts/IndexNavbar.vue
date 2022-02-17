<template>
  <nav
    class="
      navbar navbar-expand-lg navbar-dark navbar-absolute
      bg-transparent
      shadow-none
    "
    style="z-index: 1000"
  >
    <div class="container">
      <a class="navbar-brand text-white">
        <img
          src="../../assets/img/logos/white.png"
          class="nav__image"
          alt="Dzicace"
        />
      </a>
      <button
        class="navbar-togglerr"
        type="button"
        data-toggle="collapse"
        data-target="#navigation"
        aria-expanded="false"
        aria-label="Toggle navigation"
        @click="toggle"
      >
        <span>
          <i class="fa-duotone fa-bars"></i>
        </span>
      </button>
      <div class="collapse navbar-collapse" id="navbar-header-9">
        <ul class="navbar-nav me-auto ms-3">
          <li class="nav-item">
            <router-link
              :to="{ name: 'allTalents' }"
              class="nav-link text-white"
            >
              Talents
            </router-link>
          </li>
          <li class="nav-item">
            <router-link :to="{ name: 'index' }" class="nav-link text-white">
              Categories
            </router-link>
          </li>
          <li class="nav-item">
            <router-link
              class="nav-link text-white"
              :to="{ name: 'talentsJoin' }"
            >
              Join Us
            </router-link>
          </li>
        </ul>

        <ul class="nav navbar-nav d-flex" style="flex-wrap: nowrap">
          <div class="input-group mb-0 w-auto">
            <input
              type="text"
              class="form-control d-block"
              placeholder="Search..."
              autocomplete="off"
              v-model="query"
            />
            <router-link
              :to="{ name: 'allTalents', query: { search: query } }"
              class="btn btn-outline-dark mb-0"
              type="button"
            >
              <i class="fas fa-magnifying-glass"></i>
            </router-link>
          </div>
          <router-link
            :to="{ name: 'login' }"
            class="btn btn-outline-dark mb-0 ms-3"
            v-if="!isAuth"
          >
            <i class="fa-duotone fa-shield"></i>
            <span class="ms-2">LOGIN</span>
          </router-link>
          <div class="dropdown ms-3" v-else>
            <button
              class="btn btn-outline-dark mb-0"
              id="dropdownMenuLink"
              data-bs-toggle="dropdown"
            >
              <i class="fa-duotone fa-user"></i>
              <span class="ms-2">Account</span>
            </button>
            <ul class="dropdown-menu">
              <li>
                <router-link
                  class="dropdown-item"
                  :to="{ name: 'userPanelCompletedOrders' }"
                  >Profile</router-link
                >
              </li>
              <li>
                <router-link
                  class="dropdown-item"
                  :to="{ name: 'userPanelCompletedOrders' }"
                  >Orders</router-link
                >
              </li>
              <li>
                <router-link class="dropdown-item" :to="{ name: panelLink }"
                  >Saved</router-link
                >
              </li>
              <li>
                <button
                  @click="$store.dispatch('auth/logout')"
                  class="dropdown-item text-danger"
                >
                  Logout
                </button>
              </li>
            </ul>
          </div>
        </ul>
      </div>
    </div>
    <!-- MOBILE NAVBAR -->
    <div
      id="navigation"
      class="navbar-collapse w-100 pt-2 pb-2 py-lg-0 collapse"
    >
      <ul class="navbar-nav navbar-nav-hover mx-auto">
        <li class="nav-item dropdown dropdown-hover mx-2">
          <router-link
            class="
              nav-link
              ps-2
              d-flex
              justify-content-between
              cursor-pointer
              align-items-center
              text-dark
              fw-bold
            "
            :to="{ name: 'allTalents' }"
          >
            Talents
          </router-link>
        </li>
        <li class="nav-item dropdown dropdown-hover mx-2">
          <router-link
            class="
              nav-link
              ps-2
              d-flex
              justify-content-between
              cursor-pointer
              align-items-center
              text-dark
              fw-bold
            "
            :to="{ name: 'index' }"
          >
            Categories
          </router-link>
        </li>
        <li class="nav-item dropdown dropdown-hover mx-2">
          <router-link
            class="
              nav-link
              ps-2
              d-flex
              justify-content-between
              cursor-pointer
              align-items-center
              text-dark
              fw-bold
            "
            :to="{ name: 'talentsJoin' }"
          >
            Join Us
          </router-link>
        </li>
        <li class="nav-item dropdown dropdown-hover mx-2">
          <router-link
            class="
              nav-link
              ps-2
              d-flex
              justify-content-between
              cursor-pointer
              align-items-center
              text-dark
              fw-bold
            "
            :to="{ name: 'login' }"
            v-if="!isAuth"
          >
            Login
          </router-link>
          <router-link
            class="
              nav-link
              ps-2
              d-flex
              justify-content-between
              cursor-pointer
              align-items-center
              text-dark
              fw-bold
            "
            :to="{ name: panelLink }"
            v-else
          >
            Dashboard
          </router-link>
        </li>
        <li class="nav-item dropdown dropdown-hover ms-3 mx-2 mt-2 mb-2">
          <div class="input-group mb-3">
            <input
              type="text"
              class="form-control d-block"
              placeholder="Search..."
              v-model="query"
            />
            <router-link
              :to="{ name: 'allTalents', query: { search: query } }"
              class="btn btn-outline-secondary mb-0"
              type="button"
            >
              <i class="fas fa-magnifying-glass"></i>
            </router-link>
          </div>
        </li>
      </ul>
    </div>
  </nav>
</template>

<script>
export default {
  data() {
    return {
      query: "",
    };
  },
  computed: {
    isAuth() {
      return this.$store.getters["auth/isAuthenticated"];
    },
    panelLink() {
      const role = this.$store.getters["auth/getUserRole"];
      if (role === "admin") return "adminPanelTalentsPending";
      if (role === "talent") return "talentPanelProfile";
      if (role === "user") return "userPanelCompletedOrders";
      else return "";
    },
  },
  methods: {
    toggle() {
      const navigation = document.querySelector("#navigation");
      const isToggled = navigation.classList.contains("show");
      if (isToggled) {
        navigation.classList.remove("show");
        navigation.classList.remove("animation");
      } else {
        navigation.classList.add("show");
        navigation.classList.add("animation");
      }
    },
  },
  mounted() {
    if (this.$store.getters["auth/getUserRole"] === undefined && this.isAuth)
      this.$store.dispatch("auth/getUserRole");
  },
};
</script>

<style lang="scss" scoped>
.nav {
  padding: 20px 15px;
  &__image {
    width: 100px;
  }
}

.nav-link {
  font-size: 19px;
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

.btn-outline-dark {
  color: white;
  border-color: white;
  font-size: 14px;
  position: relative;
  z-index: 2000;

  &:hover {
    color: white;
    border-color: white;
  }
}

#navigation {
  background-color: #ffffff;
  border-radius: 20px;
  margin-top: 15px;
  transform: translateY(-250px);
  opacity: 0.7;
  transition: all 0.3s;
  display: block !important;
  visibility: hidden;

  @media screen and (min-width: 992px) {
    display: none !important;
  }
}
.animation {
  transform: translateY(0px) !important;
  opacity: 1 !important;
  visibility: visible !important;
}
</style>