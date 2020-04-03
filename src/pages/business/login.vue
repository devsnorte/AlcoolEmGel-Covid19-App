<template>
  <f7-page no-toolbar no-navbar no-swipeback login-screen>
    <f7-login-screen-title>Cadê o Alcool em Gel?</f7-login-screen-title>
    <f7-list form>
      <f7-list-input
        label="Email"
        type="text"
        placeholder="Digite seu email"
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
      <f7-block-footer>Problemas com seu acesso?<br>Entre em contato em danielpinon@danielpinon.com.br</f7-block-footer>
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
        app.preloader.show();
        app.request.postJSON('http://127.0.0.1:8000/api/auth/login', { email:self.username, password: self.password}, 
        // Sucess
        (data)=>{
          localStorage.setItem("key",data.access_token);
          app.views.main.router.navigate('/business/');
          app.preloader.hide();
        },
        // Error
        ()=>{
          app.preloader.hide();
          app.dialog.alert("Erro ao efetuar login, tente novamente ou entre em contato pelo email danielpinon@danielpinon.com.br");
        });
        /*
        Url:
        http://127.0.0.1:8000/api/auth/login POST
        Request:
        {
          "login":"extrafarma",
          "password":"extra123"	
        }
        Retorno:
        {
          "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC8xMjcuMC4wLjE6ODAwMFwvYXBpXC9hdXRoXC9sb2dpbiIsImlhdCI6MTU4NTQ1MzUyMywiZXhwIjoxNTg1NDU3MTIzLCJuYmYiOjE1ODU0NTM1MjMsImp0aSI6InVlQ0MzMFJQYmNBTWFBcDEiLCJzdWIiOjEsInBydiI6Ijg3ZTBhZjFlZjlmZDE1ODEyZmRlYzk3MTUzYTE0ZTBiMDQ3NTQ2YWEifQ.0BUhuppa-270RMgtu7Iuzo1iyhjnR7Ep3_7StnF3jG0",
          "token_type": "bearer",
          "expires_in": 3600
        }
        .then(function (res) {
          console.log(res.data);
          localStorage.setItem("key","dahsudahsudhasduah");
        }); */
        //this.$f7.views.main.router.navigate('/business/')
      },
      registrar(){
        this.$f7.views.main.router.navigate('/register/')
      } 
    }
  };
</script>
