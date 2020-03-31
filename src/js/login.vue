<template>
  <form class="login-form flex f-d-column a-i-center form-sturdy" v-if="f_visible" @submit.stop.prevent="submit()">
    <p class="logo-text">easypac</p>
    <div class="logo-img"><img src="/img/logo.svg" alt=""></div>
    <div class="fields-wrap flex f-d-column" v-if="!authorised">
      <p class="field">Имя пользователя</p>
      <input type="text" v-model="login">
      <p class="field">Пароль</p>
      <input type="password"v-model="password">
    </div>
    <button type="submit" name="button" class="sexy-button" v-if="!authorised">
      Войти
    </button>
    <p class="wait-for-it" v-if="authorised">Соединяемся...</p>
    <div class="wait-for-it-lines" v-if="authorised"></div>
  </form>
</template>

<script>
export default {
  name: 'login',
  props: {
    users: {
      type: Array,
      default: function() { return[{user: 'Admin', pw: 'admin'}, {user: 'User', pw: '12345'}] }
    }
  },
  data () {
    return {
      login: '',
      password: '',
      authorised: false,
      error: false,
      f_visible: true
    }
  },
  methods: {
    checkPassword: function() {
      //console.log("Checking..."+this.users[0].user);
      for(let i = 0; i < this.users.length; i++) {
        if(this.users[i].user === this.login) {
          if(this.users[i].pw === this.password) return true;
          else return false;
        }
      }
      return false;
    },
    submit: function() {
      if(this.checkPassword()) {
        this.authorised = true;
        //console.log(this.users[0].user);
        this.$router.push("/workspace/"+this.login);
      }
    }

  }
}
</script>
