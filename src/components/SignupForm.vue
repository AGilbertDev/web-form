<template>
	<form @submit.prevent="handleSubmit">
		<label> Email: </label>
		<input type="email" required autocomplete="email" v-model="email" />

		<label> Password: </label>
		<input type="password" required autocomplete="new-password" v-model="password" />
		<p v-if="passwordError !== ''" class="err">{{ passwordError }}</p>

		<label>Role:</label>
		<select v-model="role">
			<option value="developer">Web Developer</option>
			<option value="designer">Web Designer</option>
		</select>

		<label>Skills:</label>
		<input type="text" v-model="tempSkill" @keyup="addSkill" />
		<div v-for="skill in skills" :key="skill" class="pill" @click="deleteSkill(skill)">{{ skill }}</div>

		<div class="terms">
			<input type="checkbox" required v-model="terms" />
			<label>Accept terms and conditions</label>
		</div>

		<div class="submit"><button type="submit">Create an account</button></div>
	</form>
</template>

<script>
export default {
	data() {
		return {
			email: "",
			password: "",
			role: "designer",
			terms: false,
			tempSkill: "",
			skills: [],
			passwordError: ""
		};
	},
	methods: {
		addSkill(event) {
			if ((event.key === "Enter" || event.key === ",") && this.tempSkill) {
				if (!this.skills.includes(this.tempSkill.split(",")[0].trim())) {
					this.skills.push(this.tempSkill.split(",")[0].trim());
				}
				this.tempSkill = "";
			}
		},
		deleteSkill(skill) {
			this.skills = this.skills.filter((s) => s !== skill);
		},
		handleSubmit(event) {
			// validate password
			this.passwordError = this.password.length < 6 ? "Password must be at least 6 characters." : "";

			if (!this.passwordError) {
				// submit form
				console.log({
					email: this.email,
					password: this.password,
					role: this.role,
					terms: this.terms,
					skills: this.skills
				});
			}
		}
	}
};
</script>

<style scoped>
form {
	max-width: 420px;
	margin: 30px auto;
	background: white;
	text-align: left;
	padding: 40px;
	border-radius: 10px;
}
label {
	color: #aaa;
	display: inline-block;
	margin: 25px 0 15px;
	font-size: 0.6em;
	text-transform: uppercase;
	letter-spacing: 1px;
	font-weight: bold;
}
input,
select {
	display: block;
	padding: 10px 6px;
	background: white;
	width: 100%;
	box-sizing: border-box;
	border: none;
	border-bottom: 1px solid #ddd;
	color: #555;
}
input[type="checkbox"] {
	display: inline-block;
	width: 16px;
	margin: 0 10px 0 0;
	position: relative;
	top: 2px;
}
.pill {
	display: inline-block;
	margin: 20px 10px 0 0;
	padding: 6px 12px;
	background: #eee;
	border-radius: 20px;
	font-size: 12px;
	letter-spacing: 1px;
	font-weight: bold;
	color: #777;
	cursor: pointer;
}
button {
	background: #0b6dff;
	border: none;
	padding: 10px 20px;
	margin-top: 20px;
	color: white;
	border-radius: 20px;
}
.submit {
	text-align: center;
}
.err {
	color: #ff0062;
	margin-top: 10px;
	font-size: 0.8em;
	font-weight: bold;
}
</style>
