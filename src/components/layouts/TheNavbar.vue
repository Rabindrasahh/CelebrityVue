<template>
	<nav class="navbar navbar-expand-lg navbar-dark bg-transparent shadow-none">
		<div class="container">
			<router-link
				to="/"
				class="navbar-brand text-white"
			>
				<img
					src="../../assets/img/logos/black.png"
					class="nav__image"
					alt="Dzicace"
				>
			</router-link>
			<button
				class="navbar-togglerr"
				@click="toggle"
			>
				<span>
					<i class="fa-duotone fa-bars"></i>
				</span>
			</button>
			<div
				class="collapse navbar-collapse"
				id="navbar-header-9"
			>
				<ul class="navbar-nav me-auto">
					<li class="nav-item">
						<router-link
							:to="{name:'allTalents'}"
							class="nav-link text-dark"
						>
							Talents
						</router-link>
					</li>
					<li class="nav-item">
						<router-link
							:to="{name:'index'}"
							class="nav-link text-dark"
						>
							Categories
						</router-link>
					</li>
					<li class="nav-item">
						<router-link
							class="nav-link text-dark"
							:to="{name:'talentsJoin'}"
						>
							Join Us
						</router-link>
					</li>
				</ul>

				<ul
					class="nav navbar-nav d-flex"
					style="flex-wrap:nowrap"
				>
					<div class="input-group mb-0 w-auto">
						<input
							type="text"
							class="form-control d-block"
							placeholder="Search..."
							v-model="query"
						>
						<router-link
							:to="{name:'allTalents',query:{search:query}}"
							class="btn btn-outline-dark mb-0"
							type="button"
						>
							<i class="fas fa-magnifying-glass"></i>
						</router-link>
					</div>
					<router-link
						:to="{name:'login'}"
						class="btn btn-outline-dark mb-0 ms-3"
						v-if="!isAuth"
					>
						<i class="fa-duotone fa-shield"></i>
						<span class="ms-2">LOGIN</span>
					</router-link>
					<div
						class="dropdown"
						v-else
					>
						<button
							class="btn btn-outline-dark mb-0 ms-3"
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
									:to="{name:'userPanelCompletedOrders'}"
								>Profile</router-link>
							</li>
							<li>
								<router-link
									class="dropdown-item"
									:to="{name:'userPanelCompletedOrders'}"
								>Orders</router-link>
							</li>
							<li>
								<router-link
									class="dropdown-item"
									:to="{name:panelLink}"
								>Saved</router-link>
							</li>
							<li>
								<button
									@click="$store.dispatch('auth/logout')"
									class="dropdown-item text-danger"
								>Logout</button>
							</li>
						</ul>
					</div>
					<!-- <li class="nav-item">
						<a
							class="nav-link text-white"
							href="https://www.instagram.com/CreativeTimOfficial"
						>
							<i class="fab fa-instagram"></i>
						</a>
					</li> -->
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
						class="nav-link ps-2 d-flex justify-content-between cursor-pointer align-items-center text-white fw-bold"
						:to="{name:'allTalents'}"
					>
						Talents
					</router-link>
				</li>
				<li class="nav-item dropdown dropdown-hover mx-2">
					<router-link
						class="nav-link ps-2 d-flex justify-content-between cursor-pointer align-items-center text-white fw-bold"
						:to="{name:'index'}"
					>
						Categories
					</router-link>
				</li>
				<li class="nav-item dropdown dropdown-hover mx-2">
					<router-link
						class="nav-link ps-2 d-flex justify-content-between cursor-pointer align-items-center text-white fw-bold"
						:to="{name:'talentsJoin'}"
					>
						Join Us
					</router-link>
				</li>
				<li class="nav-item dropdown dropdown-hover mx-2">
					<router-link
						class="nav-link ps-2 d-flex justify-content-between cursor-pointer align-items-center text-white fw-bold"
						:to="{name:'login'}"
						v-if="!isAuth"
					>
						Login
					</router-link>
					<router-link
						class="nav-link ps-2 d-flex justify-content-between cursor-pointer align-items-center text-white fw-bold"
						:to="{name:panelLink}"
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
						>
						<router-link
							:to="{name:'allTalents',query:{search:query}}"
							class="btn btn-outline-white mb-0"
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
		if (
			this.$store.getters["auth/getUserRole"] === undefined &&
			this.isAuth
		)
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
	color: rgb(29, 29, 29) !important;
	font-weight: 600;
}
#navigation .nav-link {
	font-size: 19px;
	color: rgb(255, 255, 255) !important;
	font-weight: 600;
}

.navbar-togglerr {
	border: none;
	outline: none;
	background-color: transparent;
	font-size: 30px;
	color: rgb(29, 29, 29);

	@media screen and (min-width: 992px) {
		display: none;
	}
}

.btn-outline-dark {
	color: rgb(29, 29, 29);
	border-color: rgb(29, 29, 29);
	font-size: 14px;

	&:hover {
		color: rgb(29, 29, 29);
		border-color: rgb(29, 29, 29);
	}
}

#navigation {
	background-color: #252525;
	border-radius: 20px;
	margin-top: 15px;
	transform: translateY(-250px);
	opacity: 0.7;
	transition: all 0.3s;
	display: block !important;
	visibility: hidden;
	position: relative;
	z-index: 3000;

	// FIXED VERSION
	// position: fixed;
	// top: 45px;
	// left: 20px;
	// z-index: 3000;
	// width: 90% !important;

	@media screen and (min-width: 992px) {
		display: none !important;
	}
}
.animation {
	transform: translateY(0px) !important;
	opacity: 1 !important;
	visibility: visible !important;
}

.navbar-expand-lg {
	height: 50px !important;
	@media screen and (min-width: 998px) {
		height: 100px !important;
	}
}
</style>