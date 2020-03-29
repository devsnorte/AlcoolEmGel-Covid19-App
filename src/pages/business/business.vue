<template>
  <f7-page>
    <!-- Top Navbar -->
    <f7-navbar :sliding="false" >
      <f7-nav-left>
        <f7-link icon-ios="f7:menu" icon-aurora="f7:menu" icon-md="material:menu" panel-open="left"></f7-link>
      </f7-nav-left>
      <f7-nav-title sliding>Cadê o Alcool?</f7-nav-title>
      <f7-nav-right v-if="logaded" class="padding-right">
          Aberto &nbsp;
          <f7-toggle slot="after" :checked="data.user.aberto"></f7-toggle>
      </f7-nav-right>
    </f7-navbar>
    <!-- Pagina logada-->
    <div v-if="logaded">
      <f7-list inset>
        <f7-list-item title="Álcool em Gel 70%" name="open">
          <f7-toggle slot="after" :checked="data.itens.alcool.gel"></f7-toggle>
        </f7-list-item>
        <f7-list-item title="Álcool Líquido 70%" name="open">
          <f7-toggle slot="after" :checked="data.itens.alcool.liquido"></f7-toggle>
        </f7-list-item>
        <f7-list-item title="Máscara" name="open">
          <f7-toggle slot="after" :checked="data.itens.mascara" ></f7-toggle>
        </f7-list-item>
        <f7-list-item title="Luva" name="open">
          <f7-toggle slot="after" :checked="data.itens.luva"></f7-toggle>
        </f7-list-item>
      </f7-list>
      <!-- Mapa-->
      <l-map
        :zoom="zoom"
        :center="center"
        :options="mapOptions"
        style="height: 50vh"
        @update:center="centerUpdate"
        @update:zoom="zoomUpdate"
        ref="myMap">
        <l-tile-layer
          :url="url"
          :attribution="attribution"
        />
        <l-locatecontrol/>
        <l-marker :lat-lng="currentCenter"></l-marker>
      </l-map>
      <button class="button button-large button-raised button-fill salvar btn-ponto-mapa">Salvar ponto do mapa</button>
    </div>
    <!-- Pagina não logada-->
    <div v-else>
      <f7-block-title>Faça o login no aplicativo!</f7-block-title>
      <f7-card
        content="Faça o login clicando no botão cadeado a baixo!"
      >
      </f7-card>
      <f7-fab 
        position="right-bottom" 
        slot="fixed" 
        morph-to=".toolbar.fab-morph-target"
        @click="loginPage()">
        <f7-icon f7="lock_fill"></f7-icon>
      </f7-fab>
    </div>
  </f7-page>
</template>
<script>
// Import Vue
  import Vue from 'vue';
  // Leaflet
  import { L,latLng, DomEvent } from "leaflet"; 
  import { LMap, LTileLayer, LMarker, LPopup, LTooltip, LControl } from "vue2-leaflet"; 
  import Vue2LeafletLocatecontrol from 'vue2-leaflet-locatecontrol/Vue2LeafletLocatecontrol';
  Vue.component('l-locatecontrol', Vue2LeafletLocatecontrol)
  export default {
    components: { LMap, 
      LTileLayer, 
      LMarker, 
      LPopup, 
      LTooltip
    }, 
    data() {
      return {  
        // Informações do usuário
        content: 'hello world',
        logaded:false,
        data:{
          user:{
            aberto:true
          },
          itens:{
            alcool:{
              gel:true,
              liquido:true
            },
            mascara:true,
            luva:true
          }
        },
        // Informações do mapa
        zoom: 3, 
        center: latLng(-15.77972, -47.92972), 
        url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', 
        attribution: 'VUE.JS NORTE', 
        //withPopup: latLng(-1.45502, -48.5024), 
        withTooltip: latLng(-1.45502, -48.5024), 
        currentZoom: 11.5, 
        currentCenter: latLng(-1.45502, -48.5024), 
        showParagraph: false, 
        mapOptions: { 
          zoomSnap: 0.5
        }, 
      };
    },
    methods:{
      loginPage(){
        this.$f7.views.main.router.navigate('/login/')
      },
      // Configurações do mapa
      zoomUpdate(zoom) { 
        this.currentZoom = zoom; 
      }, 
      centerUpdate(center) { 
        this.currentCenter = center; 
      }, 
      showLongText() { 
        this.showParagraph = !this.showParagraph; 
      }, 
    },
    mounted(){
      if(localStorage.getItem("key")){
        this.logaded = true;
      }else{
        this.logaded = false;
      }
    }
  };
</script>
<style>
  .leaflet-bar a{
    width: 45px !important;
    height: 45px !important;
    line-height: 45px !important;
  }
  .btn-ponto-mapa{
    position: fixed;
    bottom: 6vh;
    z-index: 1000;
  }
</style>
