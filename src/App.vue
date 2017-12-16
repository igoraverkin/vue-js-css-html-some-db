<<template>
  <div class="login-wrap">
    <input id="tabInput" type="radio" name="tab" class="sign-in" checked><label for="tabInput" class="tab" @click="showForm(1)">Вход</label>
    <input id="tabRegistration" type="radio" name="tab" class="sign-up"><label for="tabRegistration" class="tab" @click="showForm(2)">Регистрация</label>
    <transition name="component-fade">
      <form class="sign-in-form" v-show="showSignInForm" @submit.prevent="userAccept">
        <div class="input-group">
          <label for="pass" class="label">Почта</label>
          <input id="pass" type="email" class="input" required v-model="user.email" >
        </div>
        <div class="input-group">
          <label for="pass" class="label">Пароль</label>
          <input id="pass" type="password" class="input" data-type="password" required v-model="user.password">
        </div>          
        <button type="submit">ВОЙТИ</button>
        <div class="hr"></div>
        <div class="footer-link">
          <a href="#forgot">Забыли пароль?</a>
        </div>
      </form>
    </transition>
    <transition name="component-fade">
      <form class="sign-up-form" v-show="showSignUpForm" @submit.prevent="registerUser">
        <div class="input-group">
          <label for="pass" class="label">Почта</label>
          <input id="pass" type="email" class="input" required v-model="user.email">
        </div>
        <div class="input-group">
          <label for="pass" class="label">Пароль</label>
          <input id="pass" type="password" class="input" data-type="password" required v-model="user.password">
        </div>
        <div class="input-group">
          <label for="pass" class="label">Повторить пароль</label>
          <input id="pass" type="password" class="input" data-type="password" required v-model="user.repassword">
        </div>
        <button type="submit">РЕГИСТРАЦИЯ</button>
        <div class="hr"></div>
        <div class="footer-link"@click="useRegistration">
          <label for="tabInput" >Уже зарегестрированы?</label>
        </div>
      </form>
    </transition>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {
        email: '',
        password: '',
        repassword: ''
      },
      showSignInForm: true,
      showSignUpForm: false
    }
  },
  methods: {
    showForm(num) {
      switch (num) {
        case 1:
          this.showSignInForm = true
          this.showSignUpForm = false
          break;
        case 2:
          this.showSignInForm = false
          this.showSignUpForm = true
          break;
      
        default:
          break;
      }
    },
    useRegistration() {
      this.showSignInForm = true
      this.showSignUpForm = false
    },
    registerUser(){
      if(this.user.password !== this.user.repassword || this.user.password.length <6) {
        this.error = true;
      } else{
        firebase.auth().createUserWithEmailAndPassword(this.user.email, this.user.password)
        alert('Welcome')
      }
    },
    userAccept(){
        firebase.auth().signInWithEmailAndPassword(this.user.email,this.user.password)
        .then (response => {
          const sett ={
            email:response.email,
            uid:response.uid
          }
          if(this.user.email==response.email){
            alert('Accept User');
          }
          else{
            alert('Missed');
          }
        })
    }
  }  
}
</script>

<style >

body{
  width: 100%;
  height: 100%;
  margin: -10px;
  background-color: rgb(15, 83, 139);
}

.login-wrap {
	width: 100%;
	margin: auto;
	max-width: 400px;
	min-height: 520px;
	position: relative;
	top: 200px;
	box-shadow: 0 12px 15px 0 rgba(0,0,0,0.24), 0 17px 50px 0 rgba(0,0,0,0.19);
	padding: 50px 50px 50px 50px;
	background: #9c9696fb;
}
login-wrap .component-fade-enter-active,
.login-wrap .component-fade-leave-active {
	transition: opacity 0.3s ease;
}

.login-wrap .component-fade-enter,
.login-wrap .component-fade-leave-to {
	opacity: 0;
}

.login-wrap .sign-in,
.login-wrap .sign-up {
	display: none;
}

.login-wrap .sign-in:checked + .tab,
.login-wrap .sign-up:checked + .tab {
	border-color: primary_color;
}

.login-wrap .tab {
	font-size: 18px;
	margin-right: 15px;
	padding-bottom: 5px;
	margin: 10px 10px 20px 0px;
	display: inline-block;
	border-bottom: 2px solid transparent;
	text-transform: uppercase;
}

.login-wrap .sign-in-form,
.login-wrap .sign-up-form {
	top: 120px;
	left: 50px;
	right: 50px;
	position: absolute;
}

.login-wrap .sign-in-form .input-group,
.login-wrap .sign-up-form .input-group {
	margin-bottom: 15px;
}

.login-wrap .sign-in-form .input-group .label,
.login-wrap .sign-up-form .input-group .label {
	text-transform: uppercase;
	width: 100%;
	color: #252525;
	display: block;
	font-size: 12px;
}

.login-wrap .sign-in-form .input-group .input,
.login-wrap .sign-up-form .input-group .input {
	width: 100%;
	color: default_color;
	display: block;
	border: 1px solid #ffffff;
	padding: 15px 20px;
	border-radius: 3px;
	background: #ffffff;
}

.login-wrap .sign-in-form .input-group .input:focus,
.login-wrap .sign-up-form .input-group .input:focus {
	outline: none;
	border: 1px solid green;
}

.login-wrap .sign-in-form button,
.login-wrap .sign-up-form button {
	text-transform: uppercase;
	border: none;
	padding: 15px 20px;
	border-radius: 3px;
	width: 100%;
	color: rgb(253, 253, 253);
	display: block;
	background: rgb(165, 14, 14);
}

.login-wrap .sign-in-form .hr,
.login-wrap .sign-up-form .hr {
	height: 2px;
	margin: 10px 0 10px 0;
	background: default_color;
}

.login-wrap .sign-in-form .footer-link,
.login-wrap .sign-up-form .footer-link {
	text-align: center;
}

.login-wrap .sign-in-form .footer-link a,
.login-wrap .sign-up-form .footer-link a {
	color: inherit;
	text-decoration: none;
}

.login-wrap .sign-in-form .footer-link a:hover,
.login-wrap .sign-up-form .footer-link a:hover {
	color: blue;
}

.login-wrap .sign-in-form .footer-link label,
.login-wrap .sign-up-form .footer-link label {
	cursor: pointer;
}

.login-wrap .sign-in-form .footer-link label:hover,
.login-wrap .sign-up-form .footer-link label:hover {
	color: blue;
}

</style>

