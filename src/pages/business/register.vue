<template>
  <f7-page> 
    <f7-navbar title="Registro" back-link="Back"></f7-navbar>
    <form id="myform">
      <f7-list inset >
        <f7-list-input
          label="Estabelecimento"
          type="text"
          placeholder="Nome do Estabelecimento"
          name="name"
          :value="name"
          @input="name = $event.target.value"
        ></f7-list-input>
        <f7-list-input
          label="E-mail"
          type="email"
          placeholder="E-mail do Estabelecimento"
          name="email"
          :value="email"
          @input="email = $event.target.value"
        ></f7-list-input>
        <f7-list-input
          label="Site"
          type="url"
          placeholder="URL do site do Estabelecimento"
          name="url"
          :value="url"
          @input="url = $event.target.value"
        ></f7-list-input>
        <f7-list-input
          label="Senha"
          type="password"
          placeholder="Senha"
          name="pass"
          :value="pass"
          @input="pass = $event.target.value"
        ></f7-list-input>
        <f7-list-input
          label="Usuário"
          type="text"
          placeholder="Usuário de Acesso ao aplicativo"
          name="user"
          :value="user"
          @input="user = $event.target.value"
        ></f7-list-input>

        <f7-list-item
          title="Aberto/Fechado"
          name="open" 
          >
          <f7-toggle slot="after" :checked="aberto" ref="aberto" @change="aberto = $event.target.checked"></f7-toggle>
        </f7-list-item>
        <f7-list-input
          type="textarea"
          label="Observações"
          placeholder="Observações"
          name="observacoes"
          :value="observacoes"
          @input="observacoes = $event.target.value"
        ></f7-list-input>
        <f7-list-item
          checkbox
          name="itensDisponivel"
          value="0"
          title="Álcool em Gel 70%"
          :checked="alcool_em_gel"
          @change="alcool_em_gel = $event.target.checked"
        ></f7-list-item>
        <f7-list-item
          checkbox
          name="itensDisponivel"
          value="1"
          title="Álcool Líquido 70%"
          :checked="alcool_liquido"
          @change="alcool_liquido = $event.target.checked"
        ></f7-list-item>
        <f7-list-item
          checkbox
          name="itensDisponivel"
          value="2"
          title="Máscara"
          :checked="mascara"
          @change="mascara = $event.target.checked"
        ></f7-list-item>
        <f7-list-item
          checkbox
          name="itensDisponivel"
          value="3"
          title="Luva"
          :checked="luva"
          @change="luva = $event.target.checked"
        ></f7-list-item>
      </f7-list>
      <f7-block>
        <f7-button class="col" fill @click="save">Salvar</f7-button>
      </f7-block>
    </form>
  </f7-page>
</template>
<script>
  // Import Vue
  import Vue from 'vue';
  export default {
    data() {
      return {
        name: '',
        email: '',
        url: '',
        pass: '',
        user: '',
        aberto: true,
        observacoes: '',
        alcool_em_gel: true,
        alcool_liquido: true,
        mascara: true,
        luva: true,
      };
    },
    methods:{
      elementUpdate(element){
        console.log(element);
      },
      save(){
        const self = this;
        const app = self.$f7;
        app.preloader.show();
        app.request.postJSON('http://127.0.0.1:8000/api/auth/register', { 
          name:self.name,
          email:self.email,
          user:self.user,
          alcool_em_gel:self.alcool_em_gel,
          alcool_liquido:self.alcool_liquido,
          mascara:self.mascara,
          luva:self.luva,
          aberto:self.aberto,
          obeservacao:self.obeservacao,
          url:self.url,
          password:self.pass 
        },
        //Sucesso
        (data)=>{
          localStorage.setItem("key",data.access_token);
          app.views.main.router.navigate('/business/');
          app.preloader.hide();
        },
        // Falhou
        ()=>{
          app.dialog.alert("Erro ao registrar! Entre em contato com danielpinon@danielpinon.com.br para mais informações.");
          app.preloader.hide();
        });
        /*
        Url:
        http://127.0.0.1:8000/api/auth/register POST
        Request:
        {
          
        }
        Retorno:
        {
          "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC8xMjcuMC4wLjE6ODAwMFwvYXBpXC9hdXRoXC9sb2dpbiIsImlhdCI6MTU4NTQ1MzUyMywiZXhwIjoxNTg1NDU3MTIzLCJuYmYiOjE1ODU0NTM1MjMsImp0aSI6InVlQ0MzMFJQYmNBTWFBcDEiLCJzdWIiOjEsInBydiI6Ijg3ZTBhZjFlZjlmZDE1ODEyZmRlYzk3MTUzYTE0ZTBiMDQ3NTQ2YWEifQ.0BUhuppa-270RMgtu7Iuzo1iyhjnR7Ep3_7StnF3jG0",
          "token_type": "bearer",
          "expires_in": 3600
        }
        //error
        {
          "error": "Seus dados já possuem em nossa base de dados"
        }
         
        .then(function (res) {
          console.log(res.data);
          localStorage.setItem("key","dahsudahsudhasduah");
        });
        localStorage.setItem("key","dahsudahsudhasduah");
        this.$f7.views.main.router.navigate('/business/')
        //console.log(this.$f7.$("#myform"));
        */
      }
   } 
  }
</script>