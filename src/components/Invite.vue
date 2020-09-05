<template>
	<div id="invite">
		<div class="modal" :class="{ active: inviteActive }" id="modal-id">
			<div
				@click="$emit('invite-toggle')"
				class="modal-overlay"
				aria-label="Close"
			></div>
			<div class="modal-container">
				<div class="modal-header">
					<div
						@click="$emit('invite-toggle')"
						class="btn btn-clear float-right"
						aria-label="Close"
					></div>

					<div class="modal-title h5">Invite your friends per mail</div>
					<div class="card-subtitle text-gray float-left">
						You can add emails adress of people you would like to invite to your
						activity.
					</div>
					<div class="email-container">
						<div v-for="(email, index) in emails" :key="email" class="chip">
							<img :src="email.hash" class="avatar avatar-sm" />
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
				<div class="modal-body">
					<div class="content">
						<div class="input-group">
							<input
								name="invite"
								@keyup="validateEmail"
								@blur="searching = false"
								type="text"
								class="form-input"
								:class="{ 'is-error': !validAdress }"
								v-model="email"
							/>
							<button
								:disabled="!validAdress"
								:class="{ loading: searching }"
								@click="addEmail()"
								class="btn btn-primary input-group-btn"
							>
								<i class="icon icon-plus"></i> Add email
							</button>
						</div>
						<label v-if="!validAdress" for="invite" class="invalid-mail"
							>Please enter a valid email adress</label
						>

						<div class="form-group">
							<label class="form-label" for="input-example-3"
								>You can optionally add text here for everybody.</label
							>
							<textarea
								class="form-input"
								id="input-example-3"
								placeholder="Textarea"
								rows="3"
							></textarea>
						</div>
					</div>
				</div>
				<div class="modal-footer"></div>
			</div>
		</div>
	</div>
</template>

<script>
import MD5 from 'crypto-js/md5';

export default {
	name: 'invite',

	props: {
		inviteActive: Boolean,
	},

	data() {
		return {
			searchtime: 0,
			validAdress: true,
			typing: false,
			email: '',
			emails: [],
			emailText: '',
		};
	},

	methods: {
		validateEmail() {
			// Initiall set the typing active
			this.typing = true;

			// Check for a valid email adress
			// Source: https://stackoverflow.com/questions/46155/how-to-validate-an-email-address-in-javascript#46181
			const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
			this.validAdress = re.test(String(this.email).toLowerCase());

			// As soon as the user has hit a valid adress, typing can be turned off
			if (this.validAdress) {
				this.typing = false;
			}
		},

		addEmail() {
			let adress = this.email;
			let hash = 'https://api.adorable.io/avatars/40/' + MD5(adress).toString();

			this.emails.push({ adress, hash });
			this.email = '';
			this.validAdress = true;
		},
	},
};
</script>

<style scoped>
.card-subtitle {
	margin-bottom: 1rem;
}

.email-container {
	width: 100%;
	display: flex;
	flex-direction: row;
	align-items: center;
	justify-content: center;
	flex-wrap: wrap;
}

.invalid-mail {
	color: #e85600;
	padding: 0.25rem;
}
</style>
