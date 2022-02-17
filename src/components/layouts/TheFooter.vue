<template>
	<div class="container-fluid">
		<div class="container pt-5 pb-3">
			<div class="d-flex flex-wrap">
				<div>
					<p class="h4 text-white">Do you to become a talent on Dzicace?</p>
					<router-link
						:to="{name:'talentsJoin'}"
						class="btn btn-outline-white"
					>
						<span class="icon-move-right">Join now
							<i
								class="fas fa-arrow-right ms-1"
								aria-hidden="true"
							></i>
						</span>
					</router-link>
				</div>
				<img
					src="../../assets/img/logo_white.svg"
					class="ms-auto logo"
					alt=""
				>
			</div>
			<div class="d-flex flex-wrap  mt-2">
				<div class="navigation mt-3">
					<router-link to="/">FAQ</router-link>
					<router-link to="/">Contact Us</router-link>
					<router-link to="/">About Us</router-link>
					<router-link to="/">Blog</router-link>
					<router-link to="/">Celebrities</router-link>
					<router-link to="/">Categories</router-link>
				</div>
				<div class="icons ms-auto mt-3">
					<i class="fab fa-twitter"></i>
					<i class="fab fa-instagram"></i>
					<i class="fab fa-facebook"></i>
				</div>
			</div>
			<p class="mt-2">©2021, Dzicace</p>
		</div>
	</div>
</template>

<style lang="scss" scoped>
@import "../../assets/scss/_variables.scss";

.container-fluid {
	margin-top: 100px;
	min-height: 250px;
	background-color: $color-ocean-green-2;
	color: white !important;
}

.logo {
	width: 150px;
	display: none;

	@media screen and (min-width: 768px) {
		display: block;
	}
}

.icons {
	color: white !important;
	font-size: 22px;

	i {
		transition: all 0.2s;
		cursor: pointer;

		&:not(:last-child) {
			margin-right: 10px;
		}

		&:hover {
			color: rgb(68, 68, 68);
		}
	}
}

.navigation {
	display: flex;
	flex-wrap: wrap;
	a {
		font-weight: 600;
		color: white;
		font-size: 18px;
		transition: all 0.2s;

		&:not(:last-child) {
			margin-right: 20px;
		}

		&:hover {
			color: rgb(68, 68, 68);
		}
	}
}
</style>