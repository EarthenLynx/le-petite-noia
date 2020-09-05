<template>
	<h2 class="activity-head text-center">{{ activity.activity }}</h2>

	<div class="container">
		<div class="columns">
			<div class="column col-lg-6 col-sm-12">
				<div class="activity-element">
					<p class="activity-participants">
						Participants:
					</p>
					<div class="activity-indicators">
						<i
							v-for="person in activity.participants"
							:key="person"
							class="icon icon-people"
						></i>
					</div>
				</div>
			</div>

			<div class="column col-lg-6 col-sm-12">
				<div class="activity-element">
					<p class="activity-price">
						Price:
					</p>
					<div class="activity-indicators">
						<i v-for="dollar in Math.round(activity.price) * 10" :key="dollar"
							>$</i
						>
					</div>
				</div>
			</div>
		</div>
		<div class="columns">
			<div class="column col-lg-12 col-sm-12">
				<div class="activity-element">
					<p class="activity-efford">
						Effort:
					</p>
					<progress
						class="progress"
						:value="activity.accessibility * 100"
						max="100"
					></progress>
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
	font-size: 1rem;
	background-color: #f2f2f2;
	padding: 1rem;
	
	border-radius: 0.25rem;
}

p {
	font-size: 1rem;
	margin: 0.25rem 0;
	text-align: center;
}

i {
	margin: 0 0.15rem;
}

progress.progress {
	width: 100%;
}

.activity-container {
	display: flex;
	flex-wrap: wrap;
	flex-direction: row;
}


.activity-element {
	min-height: 2rem;
	margin: 1rem 0;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
}

.activity-indicators {
	flex-direction: row;
}
</style>
