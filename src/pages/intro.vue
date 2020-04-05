<template>
  <f7-page name="intro">
    <f7-swiper pagination style="height:100vh;">
      <!-- Slide 0 -->
      <f7-swiper-slide>
        <center>
          <h2 class="padding-top">Olá, seja bem vindo ao</h2>
          <h1>"Cadê o alcool?"</h1>
          <img :src="images.logo" width="100%">
        </center>
      </f7-swiper-slide>
      <!-- Slide 1 -->
      <f7-swiper-slide>
        <center>
          <h2 class="padding-top">Em caso de algum sintoma procure ajuda e orientação médida</h2>
          <img :src="images.pano" width="100%">
        </center>
      </f7-swiper-slide>
      <!-- Slide 2 -->
      <f7-swiper-slide>
        <center class="padding-top">
          <h2 class="padding-top">Use sempre álcool em gel 70%</h2>
          <img :src="images.alcoolemgel" width="100%">
        </center>
      </f7-swiper-slide>
      <!-- Slide 3 -->
      <f7-swiper-slide>
        <center class="padding-top">
          <h2 class="padding-top">Se estiver apresentando os sintomas use mascará</h2>
          <img :src="images.maks" width="100%">
        </center>
      </f7-swiper-slide>
      <!-- Slide 3 -->
      <f7-swiper-slide>
        <center class="padding-top">
          <h2 class="padding-top">Somos um projeto <br>independente feito por <br>Daniel Pinon com o apoio da comunidade Vue Norte </h2><h3><a class="external" href="mailto:danielpinon@danielpinon.com.br">danielpinon@danielpinon.com.br</a></h3>
          <img :src="images.vuejsnorte" width="70%">
        </center>
      </f7-swiper-slide>
      <!-- Slide final -->
      <f7-swiper-slide>
        <center class="padding-top">
          <h2 class="padding-top">Lave sempre suas mãos com água e sabão</h2><h3>Escolha uma opção a baixo:</h3>
          <img :src="images.lavamao" width="60%">
        </center>
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
  import logo from '../images/Logo.png';
  import mask from '../images/002-medical mask.png';
  import alcoolemgel from '../images/005-disinfectant.png';
  import pano from '../images/006-sneeze.png';
  import lavamao from '../images/coronavirus.svg';
  import vuejsnorte from '../images/vuejs.png';
  export default {
    data() {
      return {
        images:{
          logo:logo,
          maks:mask,
          alcoolemgel:alcoolemgel,
          pano:pano,
          lavamao:lavamao,
          vuejsnorte:vuejsnorte,
        }
      };
    },
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
