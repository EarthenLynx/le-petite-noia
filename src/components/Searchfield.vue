<template>
	<button
		class="btn btn-primary"
		:disabled="searching"
		:class="{ loading: searching }"
		@click="search"
	>
		Search
	</button>
</template>

<script>
import axios from 'axios';

export default {
	props: {
		query: String,
	},

	data() {
		return {
			searching: false,
		};
	},

	methods: {
		search() {
			this.searching = true;

			axios.get(this.query).then((activity) => {
				this.$emit('search-activity', activity.data);
				this.searching = false;
			});
		},
	},
};
</script>

<style scoped>
button {
	width: 100%;
	margin: 1rem 0;
}
</style>
