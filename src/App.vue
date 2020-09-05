<template>
	<div id="app">
<!-- Toolbar -->
		<div class="container">
			<div class="columns">
				<div class="column col-lg-3 col-md-2 col-sm-1"></div>
				<div class="column">
          
					<h1>Find something to do</h1>
          <button @click="inviteActive = !inviteActive" class="btn float-right"><i class="icon icon-share"></i></button>
				</div>
				<div class="column col-lg-3 col-md-2 col-sm-1"></div>
			</div>
		</div>
<!-- / Toolbar -->

    <!-- Main container -->
		<div class="container">
			<div class="columns">
				<div class="column col-lg-3 col-md-2 col-sm-1"></div>
				<div class="column">
          <app-invite @invite-toggle="inviteActive = !inviteActive" :activity="activity" :inviteActive="inviteActive" />
					<app-activity :activity="activity" />
					<app-search :query="query" @search-activity="activity = $event" />
				</div>
				<div class="column col-lg-3 col-md-2 col-sm-1"></div>
			</div>
		</div>
    <!-- / Main container -->
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
      inviteurl: 'http://localhost:4210/api/invite',
      inviteActive: false
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
