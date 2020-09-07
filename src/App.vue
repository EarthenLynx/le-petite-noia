<template>
	<div id="app">
		<img
			@mouseenter="bubbleActive = true"
			@mouseleave="bubbleActive = false"
			:src="AppLogo"
			alt="application logo shows a purple, pretty bored octopus"
			class="app-image"
		/>

		<transition name="bubble-fade">
			<img
				:src="AppBubble"
				v-if="bubbleActive"
				alt="bubble shows the pretty bored octopus saying meh"
				class="app-bubble"
			/>
		</transition>

		<!-- Main container -->
		<div class="container">
			<div class="columns">
				<div class="column col-lg-2 col-md-2 col-sm-1"></div>
				<div class="column">
					<div class="card">
						<!-- Header section -->
						<div class="card-header">
							<div class="columns">
								<div class="column">
									<h1 class="float-left">La grande noia</h1>
								</div>
								<!-- Share button -->
								<div class="column-1">
									<button
										@click="inviteActive = !inviteActive"
										class="btn text-right"
									>
										<i class="icon icon-share"></i>
									</button>
									<!-- / Share button -->
								</div>
							</div>
							<div class="card-subtitle text-gray float-left">
								Bored? Find activities and invite your friends to them
							</div>
						</div>
						<!-- / Header section -->

						<!-- Body -->
						<div class="card-body">
							<app-invite
								@invite-toggle="inviteActive = !inviteActive"
								:activity="activity"
								:inviteActive="inviteActive"
							/>
							<app-activity :activity="activity" />
							<app-search
								:query="baseurl"
								@search-activity="activity = $event"
							/>
						</div>
						<!-- / Body -->
					</div>
					<small
						>Built on top of
						<a href="https://www.boredapi.com/">Bored API</a></small
					>
				</div>
				<div class="column col-lg-2 col-md-2 col-sm-1"></div>
			</div>
		</div>
		<!-- / Main container -->
	</div>
</template>

<script>
// Import the necessary components
import AppInvite from '@/components/Invite.vue';
import AppSearch from '@/components/Searchfield.vue';
import AppActivity from '@/components/Activity.vue';
import AppLogo from '@/assets/logo.png';
import AppBubble from '@/assets/bubble.png';

export default {
	name: 'App',

	components: {
		AppInvite,
		AppSearch,
		AppActivity,
	},

	data() {
		return {
			AppLogo,
			AppBubble,
			activity: {
				activity: '',
				type: '',
				participants: 1,
				price: 0,
				link: '',
				key: '',
				accessibility: 0,
			},
			baseurl: 'https://www.boredapi.com/api/activity',
			inviteurl: 'http://localhost:4210/api/invite',
			inviteActive: false,
			bubbleActive: false,
		};
	},
};
</script>

<style>
@import '~spectre.css/dist/spectre.min.css';
@import '~spectre.css/dist/spectre-icons.min.css';

h1 {
	font-size: 1.4rem;
}

#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	color: #2c3e50;
}

.app-image {
	display: block;
	margin: 0.25rem auto 0.5rem;
	cursor: pointer;
}

.app-bubble {
	position: absolute;
	top: 0;
	left: 39vw;
}

@media (max-width: 576px) {
	.app-image {
		height: 80px;
		width: 80px;
	}

	.app-bubble {
		display: none;
	}
}

@media (max-width: 992px) {
	.app-bubble {
		left: 34vw;
	}
}

@media (max-width: 768px) {
	.app-bubble {
		left: 30vw;
	}
}

/* Styles for the animations */
.bubble-fade-leave-active {
	transition: all 1s;
	transform: translateY(20px);
	opacity: 0;
}
</style>
