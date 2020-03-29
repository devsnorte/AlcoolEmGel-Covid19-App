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
        ></f7-list-input>
        <f7-list-input
          label="E-mail"
          type="email"
          placeholder="E-mail do Estabelecimento"
          name="email"
        ></f7-list-input>
        <f7-list-input
          label="Site"
          type="url"
          placeholder="URL do site do Estabelecimento"
          name="url"
        ></f7-list-input>
        <f7-list-input
          label="Senha"
          type="password"
          placeholder="Senha"
          name="pass"
        ></f7-list-input>
        <f7-list-input
          label="Usuário"
          type="text"
          placeholder="Usuário de Acesso ao aplicativo"
          name="user"
        ></f7-list-input>

        <f7-list-item
          title="Aberto/Fechado"
          name="open">
          <f7-toggle slot="after"></f7-toggle>
        </f7-list-item>
        <f7-list-input
          type="textarea"
          label="Observações"
          placeholder="Observações"
          name="observacoes"
        ></f7-list-input>
        <f7-list-item
          checkbox
          name="itensDisponivel"
          value="0"
          title="Álcool em Gel 70%"
        ></f7-list-item>
        <f7-list-item
          checkbox
          name="itensDisponivel"
          value="1"
          title="Álcool Líquido 70%"
        ></f7-list-item>
        <f7-list-item
          checkbox
          name="itensDisponivel"
          value="2"
          title="Máscara"
        ></f7-list-item>
        <f7-list-item
          checkbox
          name="itensDisponivel"
          value="3"
          title="Luva"
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
   methods:{
     save(){
        localStorage.setItem("key","dahsudahsudhasduah");
        this.$f7.views.main.router.navigate('/business/')
        //console.log(this.$f7.$("#myform"));
     }
   } 
  }
</script>