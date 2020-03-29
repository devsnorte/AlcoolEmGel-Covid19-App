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