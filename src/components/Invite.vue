<template>
	<div id="invite">
		<transition name="toast-fade">
			<Toast v-if="showToast" :msg="msgToast" />
		</transition>
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
						You can add emailAdresses adress of people you would like to invite
						to your activity.
					</div>
					<div class="email-container">
						<div
							v-for="(email, index) in emailAdresses"
							:key="email"
							class="chip"
						>
							<img :src="email.hash" class="avatar avatar-sm" />
							{{ email.adress }}
							<a
								@click="emailAdresses.splice(index, 1)"
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
								v-model="emailActive"
							/>
							<button
								:disabled="!validAdress"
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
								v-model="emailText"
								class="form-input"
								id="input-example-3"
								placeholder="Textarea"
								rows="3"
							></textarea>
						</div>
					</div>
				</div>
				<div class="modal-footer">
					<button
						:disabled="sending"
						:class="{ loading: sending }"
						@click="sendInvites()"
						class="btn btn-primary"
					>
						<i class="icon icon-mail"></i> Send invitations
					</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import MD5 from 'crypto-js/md5';
import axios from 'axios';
import Toast from '@/fragments/Toast';

export default {
	name: 'invite',

	components: {
		Toast,
	},

	props: {
		inviteActive: Boolean,
		activity: Object,
	},

	data() {
		return {
			showToast: false,
			msgToast: 'I am a toast',
			searchtime: 0,
			validAdress: true,
			typing: false,
			sending: false,
			emailActive: '',
			emailAdresses: [],
			emailText: 'asd',
		};
	},

	methods: {
		validateEmail() {
			// Initiall set the typing active
			this.typing = true;

			// Check for a valid email adress
			// Source: https://stackoverflow.com/questions/46155/how-to-validate-an-email-address-in-javascript#46181
			const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
			this.validAdress = re.test(String(this.emailActive).toLowerCase());

			// As soon as the user has hit a valid adress, typing can be turned off
			if (this.validAdress) {
				this.typing = false;
			}
		},

		addEmail() {
			const adress = this.emailActive;
			const hash =
				'https://api.adorable.io/avatars/40/' + MD5(adress).toString();

			this.emailAdresses.push({ adress, hash });
			this.emailActive = '';
			this.validAdress = true;
		},

		sendInvites() {
			// Set sending prop to true
			this.sending = true;

			const url = 'http://localhost:3000/sendmail';

			const payload = {
				emailAdresses: this.emailAdresses,
				activity: this.activity,
				emailText: this.emailText,
			};

			axios
				.post(url, payload)
				.then((res) => {
					this.showToast = true;
					this.sending = false;
					this.msgToast = res.data.msg;
					this.$emit('invite-toggle');
					setTimeout(() => {
						this.showToast = false;
					}, 2500);
				})
				.catch(() => {
					this.showToast = true;
					this.sending = false;
					this.msgToast = 'Something went wrong while fetching, please try again';
					setTimeout(() => {
						this.showToast = false;
					}, 2500);
				});
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
	height: 4rem;
	overflow-y: auto;
	background-color: #f2f2f2;
	border-radius: 0.25rem;
}

.invalid-mail {
	color: #e85600;
	padding: 0.25rem;
}

/* Styles for the toast transitions */
.toast-fade-enter {
  transform: translateX(-100px);
  opacity: 0;
}

.toast-fade-enter-active {
	transition: all 1s;
}

.toast-fade-leave-active {
  transform: translateX(100px);
  transition: all 1s;
  opacity: 0;
}
</style>
