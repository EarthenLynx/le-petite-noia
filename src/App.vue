<template>
	<div id="app">
		<div class="container">
			<div class="columns">
				<div class="column col-lg-2 col-sm-1"></div>
				<div class="column col-lg-8 col-sm-10">
					<h1>Find something to do</h1>
          <app-invite :activity="activity" />
					<app-activity :activity="activity" />
					<app-search :query="query" @search-activity="activity = $event" />
				</div>
				<div class="column col-lg-2 col-sm-1"></div>
			</div>
		</div>
	</div>
</template>

<script>
import AppInvite from '@/components/Invite.vue';
import AppSearch from '@/components/Searchfield.vue';
import AppActivity from '@/components/Activity.vue';

export default {
	name: 'App',

	components: {
    AppInvite,
		AppSearch,
		AppActivity,
	},

	data() {
		return {
			baseurl: 'https://www.boredapi.com/api/activity',
			activity: {
				activity: '',
				type: '',
				participants: 1,
				price: 0,
				link: '',
				key: '',
				accessibility: 0,
      },
      inviteurl: 'http://localhost:4210/api/invite'
		};
	},

	computed: {
		query() {
			let query = this.baseurl;
			return query;
		},
	},
};
</script>

<style>
@import '~spectre.css/dist/spectre.min.css';
@import '~spectre.css/dist/spectre-icons.min.css';

#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 40px;
}
</style>
