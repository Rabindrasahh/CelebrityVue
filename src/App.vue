<template>
	<router-view v-slot="{ Component }">
		<transition
			name="router"
			mode="out-in"
		>
			<component :is="Component" />
		</transition>
	</router-view>
</template>

<script>
import "bootstrap";
import "jquery";
import "@popperjs/core";
import "./assets/js/soft-ui-dashboard";

export default {
	created() {
		this.$store.dispatch("auth/autoLogin");
	},
	computed: {
		didAutoLogout() {
			return this.$store.getters["auth/didAutoLogout"];
		},
	},
	name: "App",
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;700&display=swap");
</style>

<style lang="scss">
@import url("./assets/css/soft-ui-pro.css");
@import url("./assets/css/font-awesome.min.css");
@import "./assets/scss/_variables.scss";

* {
	font-family: "Outfit", sans-serif;
}

body {
	background-color: #fbfbfb;
}

label {
	font-size: 14px;
}

tr {
	vetical-align: middle;
}

/* Router Transition  */
.router-enter-from,
.router-leave-to {
	opacity: 0;
}

.router-enter-active {
	transition: all 0.25s ease-out;
}

.router-leave-active {
	transition: all 0.25s ease-in;
}

.router-enter-to,
.router-leave-from {
	opacity: 1;
}

.bg-gradient-main {
	background-image: linear-gradient(
		310deg,
		$color-ocean-green-2,
		$color-emerald-green
	);
}
.text-emerald {
	color: $color-emerald-green !important;
}
.text-bottle {
	color: $color-bottle-green !important;
}
.btn-outline-emerald {
	color: $color-emerald-green;
	border-color: $color-emerald-green;
	box-shadow: none;
}
</style>
