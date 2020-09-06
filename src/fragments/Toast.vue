<template>
	<div class="toast">
		{{ msg }}
	</div>
</template>

<script>
export default {
	props: {
		msg: String,
	},
};
</script>

<style scoped>
.toast {
  position: absolute; 
  top: 5%; 
  right: 5%;
  width: 350px;
}
</style>
