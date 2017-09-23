<template>
  <div class="container">
    <div v-if="!$store.state.authUser">
      <h1>Login</h1>
      <form @submit.prevent="login">
        <p class="error" v-if="formError">{{ formError }}</p>
        <p><i>To login, use <b>demo</b> as email and <b>demo</b> as password.</i></p>
        <p>Email: <input type="text" v-model="formEmail" name="email" /></p>
        <p>Password: <input type="password" v-model="formPassword" name="password" /></p>
        <button type="submit">Login</button>
      </form>

      </h2>Register</h2>
      <form @submit.prevent="register">
        <p class="error" v-if="formError">{{ formError }}</p>
        <p>Email: <input type="text" v-model="formEmail" name="email" /></p>
        <p>Password: <input type="password" v-model="formPassword" name="password" /></p>
        <button type="submit">Register</button>
      </form>
    </div>

    <div v-else>
      Hello {{ $store.state.authUser.email }}!
      <pre>I am the secret content, I am shown only when the use is connected.</pre>
      <p><i>You can also refresh this page, you'll still be connected!</i></p>
      <button @click="logout">Logout</button>
    </div>
    <p><nuxt-link to="/secret">Super secret page</nuxt-link></p>
  </div>
</template>

<script>
export default {
  data () {
    return {
      formError: null,
      formEmail: '',
      formPassword: ''
    }
  },
  methods: {
    async register () {
      try {
        await this.$store.dispatch('register', {
          email: this.formEmail,
          password: this.formPassword
        })
        this.formEmail = ''
        this.formPassword = ''
        this.formError = null
      } catch (e) {
        this.formError = e.message
      }
    },

    async login () {
      try {
        await this.$store.dispatch('login', {
          email: this.formEmail,
          password: this.formPassword
        })
        this.formEmail = ''
        this.formPassword = ''
        this.formError = null
      } catch (e) {
        this.formError = e.message
      }
    },
    async logout () {
      try {
        await this.$store.dispatch('logout')
      } catch (e) {
        this.formError = e.message
      }
    }
  }
}
</script>

<style>
.container {
  padding: 100px;
}
.error {
  color: red;
}
</style>
