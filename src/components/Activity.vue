<template>
	<h2 class="activity-head">{{ activity.activity }}</h2>

	<div class="container">
		<div class="columns">
			<div class="column col-lg-4 col-sm-12">
				<div class="activity-element">
					<p class="activity-participants">
						Participants:
					</p>
					<i
						v-for="person in activity.participants"
						:key="person"
						class="icon icon-people"
					></i>
				</div>
			</div>

			<div class="column col-lg-4 col-sm-12">
				<div class="activity-element">
					<p class="activity-price">
						Price:
					</p>
					<i v-for="dollar in Math.round(activity.price) * 10 + 1" :key="dollar"
						>$</i
					>
				</div>
			</div>

			<div class="column col-lg-4 col-sm-12">
				<div class="activity-element">
					<p class="activity-price">
						Effort:
					</p>
					<i
						v-for="access in Math.round(activity.accessibility) * 10 + 1"
						:key="access"
						class="icon icon-flag"
					></i>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	props: {
		activity: Object,
	},
};
</script>

<style scoped>
h2 {
	font-size: 1.2rem;
}

p {
	font-size: 1rem;
	margin: 0;
}

i {
	margin: 0 0.15rem;
}

.activity-container {
	display: flex;
	flex-direction: row;
}
</style>
