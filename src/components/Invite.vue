<template>
	<div id="invite">
		<div class="input-group">
			<input type="text" class="form-input" v-model="email" />
			<button @click="addEmail()" class="btn btn-primary input-group-btn">
				<i class="icon icon-plus"></i> Add email
			</button>
		</div>
		<div class="email-container">
			<div v-for="(email, index) in emails" :key="email" class="chip">
        <img :src="email.hash" class="avatar avatar-sm">
				{{ email.adress }}
				<a
					@click="emails.splice(index, 1)"
					class="btn btn-clear"
					aria-label="Close"
					role="button"
				></a>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'invite',
	data() {
		return {
			email: '',
			emails: [],
		};
	},

	methods: {
		addEmail() {
			let adress = this.email;
      let hash = 'https://api.adorable.io/avatars/40/' + adress
      
      this.emails.push({adress, hash})
		},
	},
};
</script>

<style scoped>
#invite {
	margin: 1rem 0;
}

.email-container {
	width: 100%;
	display: flex;
	flex-direction: row;
	flex-wrap: wrap;
}
</style>
