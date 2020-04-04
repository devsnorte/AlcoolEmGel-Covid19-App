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
          <f7-toggle slot="after" :checked="data.user.aberto" @change="data.user.aberto = $event.target.checked; atualizaInfor()"></f7-toggle>
      </f7-nav-right>
    </f7-navbar>
    <!-- Pagina logada-->
    <div v-if="logaded">
      <f7-list inset>
        <f7-list-item title="Álcool em Gel 70%" name="open">
          <f7-toggle slot="after" :checked="data.itens.alcool.gel" @change="data.itens.alcool.gel = $event.target.checked; atualizaInfor()"></f7-toggle>
        </f7-list-item>
        <f7-list-item title="Álcool Líquido 70%" name="open">
          <f7-toggle slot="after" :checked="data.itens.alcool.liquido" @change="data.itens.alcool.liquido = $event.target.checked; atualizaInfor()"></f7-toggle>
        </f7-list-item>
        <f7-list-item title="Máscara" name="open">
          <f7-toggle slot="after" :checked="data.itens.mascara" @change="data.itens.mascara = $event.target.checked; atualizaInfor()"></f7-toggle>
        </f7-list-item>
        <f7-list-item title="Luva" name="open">
          <f7-toggle slot="after" :checked="data.itens.luva" @change="data.itens.luva = $event.target.checked; atualizaInfor()"></f7-toggle>
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
      <button class="button button-large button-raised button-fill salvar btn-ponto-mapa" @click="atualizaMapa()">Salvar ponto do mapa</button>
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
        logaded: false,
        data:{
          user:{
            aberto:false
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
      loginPage () {
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
      atualizaInfor () {
        const self = this;
        const app = self.$f7;
        app.preloader.show();
        app.request({
          url:"http://cadeoalcoolemgel.danielpinon.com.br/public/api/auth/business/update",
          method:"POST",
          headers:{
            Authorization:"Bearer " + localStorage.getItem("key")
          },
          data:{
            alcool_em_gel: self.data.itens.alcool.gel,
            alcool_liquido: self.data.itens.alcool.liquido,
            mascara: self.data.itens.mascara,
            luva: self.data.itens.luva,
            aberto: self.data.user.aberto,
          },
          success: (data) => {
            app.toast.create({
              icon: '<i class="f7-icons">floppy_disk</i>',
              text: 'Salvo com sucesso!',
              position: 'center',
              closeTimeout: 800,
            }).open();
          },
          error: () => {
            app.dialog.alert("Erro ao atualizar, tente novamente ou entre em contato pelo email danielpinon@danielpinon.com.br");
          },
          complete: () => {
            app.preloader.hide();
          },
        });
      },
      atualizaMapa(){
        const self = this;
        const app = self.$f7;        
        app.preloader.show();
        app.request({
          url:"http://cadeoalcoolemgel.danielpinon.com.br/public/api/auth/business/update",
          method:"POST",
          headers:{
            Authorization:"Bearer "+localStorage.getItem("key")
          },
          data:{
            lat: self.currentCenter.lat,
            long: self.currentCenter.lng,
          },
          success: (data) => {
            app.dialog.alert("Salvo com sucesso!");
          },
          error: () => {
            app.dialog.alert("Erro ao atualizar, tente novamente ou entre em contato pelo email danielpinon@danielpinon.com.br");
          },
          complete: () => {
            app.preloader.hide();
          },
        });
      }

    },
    mounted(){
      const self = this;
      const app = self.$f7;
      app.preloader.show();
      if(localStorage.getItem("key")){
        const key = localStorage.getItem("key");
        app.request({
          url:"http://cadeoalcoolemgel.danielpinon.com.br/public/api/auth/business",
          method:"POST",
          headers:{
            Authorization:"Bearer " + key
          },
          success: (data) => {
            data = JSON.parse(data);
            self.data.user.aberto = (data.aberto == 1 || data.aberto.localeCompare("true") == 0)?true:false;
            self.data.itens.alcool.gel = (data.alcool_em_gel == 1 || data.alcool_em_gel.localeCompare("true") == 0)?true:false;
            self.data.itens.alcool.liquido = (data.alcool_liquido == 1 || data.alcool_liquido.localeCompare("true") == 0)?true:false;
            self.data.itens.mascara = (data.mascara == 1 || data.mascara.localeCompare("true") == 0)?true:false;
            self.data.itens.luva = (data.luva == 1 || data.luva.localeCompare("true") == 0)?true:false;
            if(data.lat != null && data.long != null){
              console.log("True");
            }
            self.logaded = true;

            this.$nextTick(function () {
              const leafletControl = document.getElementsByClassName('leaflet-control-attribution');
              if (leafletControl != null && leafletControl.length > 0) {
                const leafletControlLink = leafletControl[0].getElementsByTagName('a');
                if (leafletControlLink != null && leafletControlLink.length > 0) {
                  document.getElementsByClassName('leaflet-control-attribution')[0].getElementsByTagName('a')[0].className = "external";
                }
              }
            })
          },
          error: () => {
            app.views.main.router.navigate('/login/');
          },
          complete: () => {
            app.preloader.hide();
          },
        });
      }else{
        app.preloader.hide();
        this.logaded = false;
        app.views.main.router.navigate('/login/');
      }
    }
  };
</script>

<style scoped>
.button {
  margin-left: 2%;
  margin-right: 2%;
  width: 96%;
}

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

<!--
  Atualização de dados

  Url:
  http://127.0.0.1:8000/api/auth/business/update POST
  Header: 
  Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC8xMjcuMC4wLjE6ODAwMFwvYXBpXC9hdXRoXC9sb2dpbiIsImlhdCI6MTU4NTQ1MzUyMywiZXhwIjoxNTg1NDU3MTIzLCJuYmYiOjE1ODU0NTM1MjMsImp0aSI6InVlQ0MzMFJQYmNBTWFBcDEiLCJzdWIiOjEsInBydiI6Ijg3ZTBhZjFlZjlmZDE1ODEyZmRlYzk3MTUzYTE0ZTBiMDQ3NTQ2YWEifQ.0BUhuppa-270RMgtu7Iuzo1iyhjnR7Ep3_7StnF3jG0
  Content-Type: application/json
  Request:
  {
    "alcool_em_gel":"true",
    "alcool_liquido":"true",
    "mascara":"true",
    "luva":"true",
    "aberto":"true",
    "lat":"",
    "long":""
  }
  Retorno:
  {
    "message": "Salvo com sucesso"
  }
  //error
  {
    "error": "Seus dados já possuem em nossa base de dados"
  }

-->