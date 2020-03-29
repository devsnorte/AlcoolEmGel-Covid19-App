<template>
  <f7-page name="intro">
    <!-- Top Navbar
    <f7-navbar :sliding="false" >
      <f7-nav-left>
        <f7-link icon-ios="f7:menu" icon-aurora="f7:menu" icon-md="material:menu" panel-open="left"></f7-link>
      </f7-nav-left>
      <f7-nav-title sliding>Cadê o Alcool?</f7-nav-title>
    </f7-navbar> -->
    <f7-swiper pagination style="height:100vh;">
      <f7-swiper-slide>
        <center>
          <h2 class="padding-top">Olá, seja bem vindo ao</h2>
          <h1>"Cadê o alcool?"</h1>
        </center>
      </f7-swiper-slide>
      <f7-swiper-slide>Slide 2</f7-swiper-slide>
      <f7-swiper-slide>
        <f7-block>
          <f7-row>
            <f7-col width="100">
              <button class="col button button-large button-raised button-fill" @click="pageClient()">
                Sou cliente
              </button>
            </f7-col>
            <f7-col width="100">
              <br>
            </f7-col>
            <f7-col width="100">
              <button class="col button button-large button-raised button-fill" @click="pageBusiness()">
                Sou estabelecimento
              </button>
            </f7-col>
          </f7-row>
        </f7-block>
      </f7-swiper-slide>
    </f7-swiper>
  </f7-page>
</template>
<script>
  export default {
    methods:{
      pageClient(){
        localStorage.setItem("intro","0")
        this.$f7.views.main.router.navigate('/')
      },
      pageBusiness(){
        localStorage.setItem("intro","1")
        this.$f7.views.main.router.navigate('/business/')
      }
    },
    mounted(){
      if(localStorage.getItem("intro") == "0"){
        this.$f7.views.main.router.navigate('/')
      }else if(localStorage.getItem("intro") == "1"){
        this.$f7.views.main.router.navigate('/business/')
      }
    }
  };
</script>
