<template>
	<div class="login-register">
		<div class="contain">
			<div class="big-box" :class="{ active: isLogin }">
				<div class="big-contain" key="bigContainLogin" v-if="isLogin">
					<div class="btitle">账户登录</div>
					<div class="bform">
						<input type="usaername" placeholder="用户名" v-model="form.username">
						<span class="errTips" v-if="emailError">* 邮箱填写错误 *</span>
						<input type="password" placeholder="密码" v-model="form.userpwd">
						<span class="errTips" v-if="emailError">* 密码填写错误 *</span>
					</div>
					<button class="bbutton" @click="login">登录</button>
				</div>
				<div class="big-contain" key="bigContainRegister" v-else>
					<div class="btitle">创建账户</div>
					<div class="bform">
						<input type="text" placeholder="用户名" v-model="form.username">
						<span class="errTips" v-if="existed">* 用户名已经存在！ *</span>
						<input type="password" placeholder="密码" v-model="form.userpwd">
						<input type="password" placeholder="确认密码" v-model="form.userpwd2">
					</div>
					<button class="bbutton" @click="register">注册</button>
				</div>
			</div>
			<div class="small-box" :class="{ active: isLogin }">
				<div class="small-contain" key="smallContainRegister" v-if="isLogin">
					<div class="stitle">你好，朋友!</div>
					<p class="scontent">开始注册，和我们一起旅行</p>
					<button class="sbutton" @click="changeType">注册</button>
				</div>
				<div class="small-contain" key="smallContainLogin" v-else>
					<div class="stitle">欢迎回来!</div>
					<p class="scontent">与我们保持联系，请登录你的账户</p>
					<button class="sbutton" @click="changeType">登录</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import axios from "axios";

export default {

	name: 'login-register',
	data() {
		return {
			isLogin: true,
			emailError: false,
			passwordError: false,
			existed: false,
			form: {
				username: '',
				userpwd: '',
				userpwd2: ''
			}
		}
	},
	methods: {
		changeType() {
			this.isLogin = !this.isLogin
			this.form.username = ''
			this.form.userpwd = ''
			this.form.userpwd2 = ''
		},

		login() {
			const self = this;
			// console.log(self.form.username)
			const formData = new FormData();
			formData.append('username', self.form.username);
			formData.append('userpwd', self.form.userpwd);
			axios.post('http://localhost:8081/users/login', formData, {
				headers: {
					'Content-Type': 'multipart/form-data'
				}
			})
				.then(res => {
					console.log(res);
					// console.log(res.data.data);
					if (res.data.flag === false) {
						// console.log(res.data.data)
						alert(res.data.data);
					} else {
						console.log(res.data.data);
						const username = res.data.data
						this.$cookies.set('userId', res.data.data.id)
						this.$store.dispatch('asyncUpdateUser', res.data.data)
						this.$router.push({ path: '/home' })
					}
				})
		},

		register() {
			const formData = new FormData();
			const self = this;
			formData.append('username', self.form.username);
			formData.append('userpwd', self.form.userpwd);
			if (self.form.username != "" && self.form.userpwd != "" && self.form.userpwd2 != "") {
				if (self.form.userpwd === self.form.userpwd2) {
					axios.post('http://localhost:8081/users/register', formData, {
						headers: {
							'Content-Type': 'multipart/form-data'
						}
					})
						.then(res => {
							if (res.data.flag === true) {
								alert('注册成功，请返回登陆页面登陆')
								changeType()
							} else {
								alert('注册失败,' + res.data.data)
							}

						})

				} else {
					alert("两次密码不一致")
				}
			} else {
				alert("填写不能为空")

			}
		}
	}
}
</script>

<style scoped="scoped">
.login-register {
	width: 100vw;
	height: 100vh;
	box-sizing: border-box;
	box-shadow: 0 0 5px 1px #999;
}

.contain {
	width: 60%;
	height: 60%;
	position: relative;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	background-color: #fff;
	border-radius: 20px;
	box-shadow: 0 0 3px #f0f0f0,
		0 0 6px #f0f0f0;
}

.big-box {
	width: 70%;
	height: 100%;
	position: absolute;
	top: 0;
	left: 30%;
	transform: translateX(0%);
	transition: all 1s;
}

.big-contain {
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.btitle {
	font-size: 1.5em;
	font-weight: bold;
	color: rgb(57, 167, 176);
}

.bform {
	width: 100%;
	height: 40%;
	padding: 2em 0;
	display: flex;
	flex-direction: column;
	justify-content: space-around;
	align-items: center;
}

.bform .errTips {
	display: block;
	width: 50%;
	text-align: left;
	color: red;
	font-size: 0.7em;
	margin-left: 1em;
}

.bform input {
	width: 50%;
	height: 50px;
	border: none;
	outline: none;
	border-radius: 10px;
	padding-left: 2em;
	background-color: #f0f0f0;
}

.bbutton {
	width: 20%;
	height: 40px;
	border-radius: 24px;
	border: none;
	outline: none;
	background-color: rgb(57, 167, 176);
	color: #fff;
	font-size: 0.9em;
	cursor: pointer;
}

.small-box {
	width: 30%;
	height: 100%;
	background: linear-gradient(135deg, rgb(57, 167, 176), rgb(56, 183, 145));
	position: absolute;
	top: 0;
	left: 0;
	transform: translateX(0%);
	transition: all 1s;
	border-top-left-radius: inherit;
	border-bottom-left-radius: inherit;
}

.small-contain {
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.stitle {
	font-size: 1.5em;
	font-weight: bold;
	color: #fff;
}

.scontent {
	font-size: 0.8em;
	color: #fff;
	text-align: center;
	padding: 2em 4em;
	line-height: 1.7em;
}

.sbutton {
	width: 60%;
	height: 40px;
	border-radius: 24px;
	border: 1px solid #fff;
	outline: none;
	background-color: transparent;
	color: #fff;
	font-size: 0.9em;
	cursor: pointer;
}

.big-box.active {
	left: 0;
	transition: all 0.5s;
}

.small-box.active {
	left: 100%;
	border-top-left-radius: 0;
	border-bottom-left-radius: 0;
	border-top-right-radius: inherit;
	border-bottom-right-radius: inherit;
	transform: translateX(-100%);
	transition: all 1s;
}
</style>
