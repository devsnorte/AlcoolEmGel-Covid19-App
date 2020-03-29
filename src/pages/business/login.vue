<template>
  <f7-page no-toolbar no-navbar no-swipeback login-screen>
    <f7-login-screen-title>Cadê o Alcool em Gel?</f7-login-screen-title>
    <f7-list form>
      <f7-list-input
        label="Usuário"
        type="text"
        placeholder="Digite seu usuário"
        :value="username"
        @input="username = $event.target.value"
      ></f7-list-input>
      <f7-list-input
        label="Senha"
        type="password"
        placeholder="Digite sua senha"
        :value="password"
        @input="password = $event.target.value"
      ></f7-list-input>
    </f7-list>
    <f7-list>
      <f7-list-button @click="login">Acessar</f7-list-button>
      <f7-list-button @click="registrar">Registrar</f7-list-button>
      <f7-block-footer>Problemas com seu acesso?<br>Entre em contato em cadeoalcoolemgel@backbonedevs.tech</f7-block-footer>
    </f7-list>
  </f7-page>
</template>
<script>
  export default {
    data() {
      return {
        username: '',
        password: '',
      };
    },
    methods:{
      login(){
        const self = this;
        const app = self.$f7;
        //alert(`Username: ${self.username}<br>Password: ${self.password}`)
        this.$app.request.promise.postJSON('http://api.myapp.com', { login:'${self.username}', password: '${self.password}' })
        .then(function (res) {
          console.log(res.data);
          localStorage.setItem("key","dahsudahsudhasduah");
        });
        //this.$f7.views.main.router.navigate('/business/')
      },
      registrar(){
        this.$f7.views.main.router.navigate('/register/')
      } 
    }
  };
</script>
