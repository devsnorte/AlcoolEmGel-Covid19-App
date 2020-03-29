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
      <l-marker :lat-lng="withTooltip" @click="innerClick"></l-marker>
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
            <f7-col width="100">Farmacia</f7-col> 
            <f7-col width="100">ExtraFarma</f7-col>
            <f7-col width="100">
              <span class="badge color-red">Fechado</span>
              <span class="badge color-green">Aberto</span>
            </f7-col>
          </f7-row>
        </f7-block-title>
        <f7-block>
          <i class="f7-icons size-22 marker-red">circle</i> Álcool em Gel 70%<br>
          <i class="f7-icons size-22 marker-red">circle</i> Álcool Líquido 70%<br>
          <i class="f7-icons size-22 marker-green">circle_fill</i> Máscara<br>
          <i class="f7-icons size-22 marker-green">circle_fill</i> Luva<br>
        </f7-block>
      </f7-page-content>
    </f7-sheet>
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
        showMap: true
      }; 
    }, 
    methods: { 
      zoomUpdate(zoom) { 
        this.currentZoom = zoom; 
      }, 
      centerUpdate(center) { 
        this.currentCenter = center; 
      }, 
      showLongText() { 
        this.showParagraph = !this.showParagraph; 
      }, 
      innerClick() { 
        const self = this;
        const $ = self.$$;
        self.sheet = self.$f7.sheet.open(".demo-sheet-swipe-to-close")
      },
      getPosition(){
        navigator.geolocation.watchPosition((position)=>{
          this.$root.$setState({ currentCenter: latLng(position.coords.latitude, position.coords.longitude)})
        });
      }
    },
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