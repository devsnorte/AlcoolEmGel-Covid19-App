<template>
  <f7-page name="home">
    <!-- Top Navbar -->
    <f7-navbar :sliding="false" >
      <f7-nav-left>
        <f7-link icon-ios="f7:menu" icon-aurora="f7:menu" icon-md="material:menu" panel-open="left"></f7-link>
      </f7-nav-left>
      <f7-nav-title sliding>Cadê o Alcool?</f7-nav-title>
      <!--<f7-nav-right>
        <f7-link class="searchbar-enable" data-searchbar=".searchbar-demo" icon-ios="f7:search" icon-aurora="f7:search" icon-md="material:search"></f7-link>
      </f7-nav-right>
      <f7-searchbar
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
      <l-marker :lat-lng="withPopup">
        <l-popup>
          <div @click="innerClick">
            I am a popup
            <p v-show="showParagraph">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque
              sed pretium nisl, ut sagittis sapien. Sed vel sollicitudin nisi.
              Donec finibus semper metus id malesuada.
            </p>
          </div>
        </l-popup>
      </l-marker>
      <l-marker :lat-lng="withTooltip">
        <l-tooltip :options="{ permanent: true, interactive: true }">
          <div @click="innerClick">
            I am a tooltip
            <p v-show="showParagraph">
              Lorem ipsum dolor sit amet, consectetur adipiscing elit. Quisque
              sed pretium nisl, ut sagittis sapien. Sed vel sollicitudin nisi.
              Donec finibus semper metus id malesuada.
            </p>
          </div>
        </l-tooltip>
      </l-marker>
    </l-map>
     <!-- Swipe to close demo sheet -->
    <f7-sheet
      class="demo-sheet-swipe-to-close"
      style="height:auto; --f7-sheet-bg-color: #fff;"
      swipe-to-close
      backdrop
    >
      <f7-page-content>
        <f7-block-title large>
          Farmacia ExtraFarma 
          <span class="badge color-red">Fechado</span>
          <span class="badge color-green">Aberto</span> 
        </f7-block-title>
        <f7-block>
          <i class="f7-icons size-22 color-red">circle</i> Álcool em Gel<br>
          <i class="f7-icons size-22">circle_fill</i> Máscara<br>
          <i class="f7-icons size-22">circle_fill</i> Luva<br>
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
  import 'leaflet/dist/leaflet.css';
  // first we import super class
  //
  
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
        zoom: 13, 
        center: latLng(47.41322, -1.219482), 
        url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', 
        attribution: 'VUE.JS NORTE', 
        withPopup: latLng(47.41322, -1.219482), 
        withTooltip: latLng(47.41422, -1.250482), 
        currentZoom: 11.5, 
        currentCenter: latLng(47.41322, -1.219482), 
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