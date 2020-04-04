<template>
  <f7-page name="home">
    <!-- Top Navbar -->
    <f7-navbar :sliding="false" >
      <f7-nav-left>
        <f7-link icon-ios="f7:menu" icon-aurora="f7:menu" icon-md="material:menu" panel-open="left"></f7-link>
      </f7-nav-left>
      <f7-nav-title sliding>Cadê o Alcool em Gel?</f7-nav-title>
      <f7-nav-right>
        <!--<f7-link class="searchbar-enable" data-searchbar=".searchbar-demo" icon-ios="f7:search" icon-aurora="f7:search" icon-md="material:search"></f7-link>-->
      </f7-nav-right>
      <!--<f7-searchbar
        class="searchbar-demo"
        expandable
        search-container=".search-list"
        search-in=".item-title"
        :disable-button="!$theme.aurora"
      ></f7-searchbar>-->
    </f7-navbar>
    <!-- Page content-->
    <l-map
      v-if="showMap"
      :zoom="zoom"
      :center="center"
      :options="mapOptions"
      style="height: 100%"
      @update:center="centerUpdate"
      @update:zoom="zoomUpdate"
      ref="myMap"
    >
      <l-tile-layer
        :url="url"
        :attribution="attribution"
      />
      <l-marker v-for="(item, index) in lojas" :key="item.id"
        :lat-lng="latitudelongtude(item.lat, item.long)" 
        @click="innerClick(index, item.id)">
      </l-marker>
      <l-locatecontrol/>
    </l-map>
     <!-- Swipe to close demo sheet -->
    <f7-sheet
      class="demo-sheet-swipe-to-close"
      style="height:auto; --f7-sheet-bg-color: #fff;"
      swipe-to-close
      backdrop>
      <div class="swipe-handler"></div>
      <f7-page-content>
        <f7-block-title large>
          <f7-row>
            <f7-col width="100">{{loja.nome}}</f7-col> 
            <f7-col width="100">
              <span class="badge color-green" v-if="loja.aberto">Aberto</span>
              <span class="badge color-red" v-else>Fechado</span>
            </f7-col>
          </f7-row>
        </f7-block-title>
        <f7-block>
          <i class="f7-icons size-22 marker-green" v-if="loja.alcool_em_gel">circle_fill</i> 
          <i class="f7-icons size-22 marker-red" v-else>circle_fill</i> 
          Álcool em Gel 70%<br>
          <i class="f7-icons size-22 marker-green" v-if="loja.alcool_liquido">circle_fill</i> 
          <i class="f7-icons size-22 marker-red" v-else>circle_fill</i> 
          Álcool Líquido 70%<br>
          <i class="f7-icons size-22 marker-green" v-if="loja.mascara">circle_fill</i> 
          <i class="f7-icons size-22 marker-red" v-else>circle_fill</i> 
          Máscara<br>
          <i class="f7-icons size-22 marker-green" v-if="loja.luva">circle_fill</i> 
          <i class="f7-icons size-22 marker-red" v-else>circle_fill</i> 
          Luva<br>
          <f7-card>
            <f7-card-content>
              Observações:<br>
              {{loja.obeservacao}}
            </f7-card-content>
          </f7-card>
          <f7-button fill @click="openSite(loja.url)">Abrir Site</f7-button>
        </f7-block>
      </f7-page-content>
    </f7-sheet>
  </f7-page>
</template>

<script>
  import { Device }  from 'framework7/framework7-lite.esm.bundle.js';
  // Import Vue
  import Vue from 'vue';
  // Leaflet
  import { L,latLng, DomEvent } from "leaflet"; 
  import { LMap, LTileLayer, LMarker, LPopup, LTooltip, LControl } from "vue2-leaflet"; 
  import Vue2LeafletLocatecontrol from 'vue2-leaflet-locatecontrol/Vue2LeafletLocatecontrol';
  Vue.component('l-locatecontrol', Vue2LeafletLocatecontrol);
  export default  { 
    name: "Example", 
    components: { LMap, 
      LTileLayer, 
      LMarker, 
      LPopup, 
      LTooltip
    }, 
    data() { 
      return { 
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
        showMap: true,
        lojas:[],
        loja:{
          nome:"",
          email:"",
          aberto:false,
          alcool_em_gel:false,
          alcool_liquido:false,
          mascara:false,
          luva:false,
          obeservacao:"",
          url:"",
        }
      }; 
    }, 
    methods: { 
      latitudelongtude(lat,long){
        return latLng(lat, long);
      },
      zoomUpdate(zoom) { 
        this.currentZoom = zoom; 
      }, 
      centerUpdate(center) { 
        this.currentCenter = center; 
      }, 
      showLongText() { 
        this.showParagraph = !this.showParagraph; 
      }, 
      innerClick(id, itemId) { 
        const self = this;
        const $ = self.$$;
        const clicked = self.lojas[id];
        const loja = self.loja;
        loja.nome = clicked.name;
        loja.aberto = (clicked.aberto.localeCompare("true") == 0)?true:false;
        loja.alcool_em_gel = (clicked.alcool_em_gel.localeCompare("true") == 0)?true:false;
        loja.alcool_liquido = (clicked.alcool_liquido.localeCompare("true") == 0)?true:false;
        loja.mascara = (clicked.mascara.localeCompare("true") == 0)?true:false;
        loja.luva = (clicked.luva.localeCompare("true") == 0)?true:false;
        loja.obeservacao = clicked.obeservacao;
        loja.url = clicked.url;
        // Abrir modal
        self.sheet = self.$f7.sheet.open(".demo-sheet-swipe-to-close")
      },
      openSite(url){
        console.log(url);
        var dispositivo = "ANDROID";
        // Handle click events for all external URLs
        if (dispositivo === 'ANDROID') {
          navigator.app.loadUrl(url, { openExternal: true });
        }
        else if (dispositivo === 'IOS') {
          window.open(url, '_system');
        }
      }
    },
    mounted (){
      const self = this;
      const app = self.$f7;
      app.preloader.show();
      app.request.post('http://cadeoalcoolemgel.danielpinon.com.br/public/api/auth/guest', 
      function (data) {
        app.preloader.hide();
        data = JSON.parse(data);
        console.log(data == []);
        if(data.length == 0){
          app.dialog.alert('Sem estabelecimentos cadastrados! Divulgue para facilitar a entrada de novos estabelecimentos.',()=>{
            self.openSite('https://api.whatsapp.com/send?text=Você conhece o aplicativo cade o alcool em gel? Entra no link bit.ly/cadeoalcoolemgel e baixe o app.');
          });
        }else{
          self.lojas = data;
        }
      },
      function (data){
        app.dialog.alert('Erro ao capturar informações do servidor!');
        app.preloader.hide();
      });                 
    }
}; 
</script>
<style>
  @import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";
  .marker-green{
    color:green !important;
  }
  .marker-red{
    color:red !important;
  }
  .swipe-handler {
    height: 16px;
    position: absolute;
    left: 0;
    width: 100%;
    top: 0;
    background: #fff;
    cursor: pointer;
    z-index: 10;
  }
  .swipe-handler::after {
    content: '';
    width: 36px;
    height: 6px;
    position: absolute;
    left: 50%;
    top: 50%;
    margin-left: -18px;
    margin-top: -3px;
    border-radius: 3px;
    background: #666;
  }
  .leaflet-bar a{
    width: 45px !important;
    height: 45px !important;
    line-height: 45px !important;
  }
</style>

<!-- 
  Request 
  Url:http://127.0.0.1:8000/api/auth/guest GET
  {
    "nome":""
    "alcool_em_gel":"true",
    "alcool_liquido":"true",
    "mascara":"true",
    "luva":"true",
    "aberto":"true",
    "lat":"",
    "long":""
  }

-->