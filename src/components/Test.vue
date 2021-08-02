<template>
	<div>
		<h1>Test Page</h1>
		<h1>{{ msg }}</h1>
		<input v-model="msg">
		<br>
		<button onclick="alert('signed up')">
		submit
	</button>
	</div>
</template>

<script>
export default {
	name: "Test",
	props:{
		msg: String,
	},
}
</script>
<style scoped>
h1 {
	margin: 40px 0 0;
}
</style>