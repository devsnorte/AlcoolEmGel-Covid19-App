<template>
<f7-app :params="f7params" >

  <f7-panel left cover>
    <f7-view>
      <f7-page>
        <f7-navbar title="Menu"></f7-navbar>
        <f7-block>
          <h2>Seja bem vindo!</h2>
        </f7-block>
        <f7-list>
          <f7-list-item link="/about/" title="Sobre o Aplicativo"></f7-list-item>
          <f7-list-item external link="https://github.com/vuejs-norte/AlcoolEmGel-Covid19-App" title="Código Fonte">
            <f7-icon slot="after" f7="logo_github"></f7-icon>
          </f7-list-item>
        </f7-list>          
      </f7-page>
    </f7-view>
  </f7-panel>

  <f7-view main class="safe-areas" url="/intro/"></f7-view>

  <f7-popup id="my-popup">
    <f7-view>
      <f7-page>
        <f7-navbar title="Popup">
          <f7-nav-right>
            <f7-link popup-close>Fechar</f7-link>
          </f7-nav-right>
        </f7-navbar>
        <f7-block>
          <p>Popup content goes here.</p>
        </f7-block>
      </f7-page>
    </f7-view>
  </f7-popup>

  <f7-login-screen id="my-login-screen">
    <f7-view>
      <f7-page login-screen>
        <f7-login-screen-title>Login</f7-login-screen-title>
        <f7-list form>
          <f7-list-input
            type="text"
            name="username"
            placeholder="Your username"
            :value="username"
            @input="username = $event.target.value"
          ></f7-list-input>
          <f7-list-input
            type="password"
            name="password"
            placeholder="Your password"
            :value="password"
            @input="password = $event.target.value"
          ></f7-list-input>
        </f7-list>
        <f7-list>
          <f7-list-button title="Sign In" @click="alertLoginData"></f7-list-button>
          <f7-block-footer>
            Some text about login information.<br>Click "Sign In" to close Login Screen
          </f7-block-footer>
        </f7-list>
      </f7-page>
    </f7-view>
  </f7-login-screen>
</f7-app>
</template>
<script>
  import { Device, Dom7 }  from 'framework7/framework7-lite.esm.bundle.js';
  import cordovaApp from '../js/cordova-app.js';
  import routes from '../js/routes.js';
  import { request } from 'framework7-vue';
  export default {
    data() {
      return {
        f7params: {
          id: 'tech.backbonedevs.cadeoalcoolemgel', // App bundle ID
          name: 'Onde Tem Alcool em Gel', // App name
          theme: 'auto', // Automatic theme detection
          routes: routes,
          input: {
            scrollIntoViewOnFocus: Device.cordova && !Device.electron,
            scrollIntoViewCentered: Device.cordova && !Device.electron,
          },
          statusbar: {
            iosOverlaysWebView: true,
            androidOverlaysWebView: false,
            androidBackgroundColor:"#f3f3f3",
            androidTextColor:"black"
          },
        },
        
        // Login screen data
        username: '',
        password: '',
      }
    },
    methods: {
      alertLoginData() {
        this.$f7.dialog.alert('Username: ' + this.username + '<br>Password: ' + this.password, () => {
          this.$f7.loginScreen.close();
        });
      }
    },
    mounted() {
      this.$f7ready((f7) => {
        if (Device.cordova) {
          cordovaApp.init(f7);
        }
      });
    }
  }
</script>