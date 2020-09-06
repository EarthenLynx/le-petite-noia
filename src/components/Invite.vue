<template>
	<div id="invite">
		<!-- Interactive Toast -->
		<transition name="toast-fade">
			<Toast v-if="showToast" :msg="msgToast" />
		</transition>
		<!-- / Interactive Toast -->

		<!-- Modal Element -->
		<div class="modal" :class="{ active: inviteActive }" id="modal-id">
			<div
				@click="$emit('invite-toggle')"
				class="modal-overlay"
				aria-label="Close"
			></div>
			<div class="modal-container">
				<!-- Modal Header -->
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
							:key="email"
							v-for="(email, index) in emailAdresses"
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
				<!-- / Modal header -->

				<!-- Modal Body -->
				<div class="modal-body">
					<div class="content">
						<div class="input-group">
							<input
								@keyup="validateEmail"
								@blur="searching = false"
								:class="{ 'is-error': !validAdress }"
								name="invite"
								type="text"
								class="form-input"
								v-model="emailActive"
							/>
							<button
								@click="addEmail()"
								:disabled="!validAdress"
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
								id="input-example-3"
								class="form-input"
								placeholder="Write a message to your fellows to join up"
								rows="3"
							></textarea>
						</div>
					</div>
				</div>
				<!-- / Modal body -->

				<!-- Modal footer -->
				<div class="modal-footer">
					<button
						@click="sendInvites()"
						:disabled="sending"
						:class="{ loading: sending }"
						class="btn btn-primary"
					>
						<i class="icon icon-mail"></i> Send invitations
					</button>
				</div>
				<!-- / Modal footer -->
			</div>
		</div>
		<!-- / Modal element -->
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
			validAdress: true,
			sending: false,
			searchtime: 0,
			emailAdresses: [],
			emailActive: '',
			emailText: '',
			msgToast: '',
		};
	},

	methods: {
		validateEmail() {
			// Check for a valid email adress
			// Source: https://stackoverflow.com/questions/46155/how-to-validate-an-email-address-in-javascript#46181
			const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
			this.validAdress = re.test(String(this.emailActive).toLowerCase());
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

			const url = 'http://localhost:4210/sendmail';

			const payload = {
				emailAdresses: this.emailAdresses,
				activity: this.activity,
				emailText: this.emailText,
			};

			axios
				// Send the adresses & content to the server
				.post(url, payload)

				// Receive a response and change the interactive DOM elements
				.then((res) => {
					this.showToast = true;
					this.sending = false;
					this.msgToast = res.data.msg;
					this.$emit('invite-toggle');
					setTimeout(() => {
						this.showToast = false;
					}, 2500);
				})

				// If something goes wrong, notify the user
				.catch(() => {
					this.showToast = true;
					this.sending = false;
					this.msgToast =
						'Something went wrong while fetching, please try again';
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
