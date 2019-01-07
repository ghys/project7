<template>
  <!-- App -->
  <f7-app :params="f7params">

    <!-- Statusbar -->
    <f7-statusbar></f7-statusbar>

    <!-- Left Panel -->
    <f7-panel left cover style="background: #fafafa">
      <!-- <f7-view url="/panel-left/"></f7-view> -->
      <div class="logo">
        <img src="static/img/openhab-logo.png" width="100%">
      </div>
      <f7-list>
        <f7-list-item link="/" title="Home" view="#main-view" panel-close>
          <f7-icon slot="media" ios="f7:home" md="material:home"></f7-icon>
        </f7-list-item>
      </f7-list>
      <f7-block-title>Sitemaps</f7-block-title>
      <f7-list>
        <f7-list-item v-for="sitemap in sitemaps" :key="sitemap.name" :link="'/sitemap/' + sitemap.name + '/' + sitemap.name" :title="sitemap.label" view="#main-view" panel-close></f7-list-item>
        <!-- <f7-list-item link="/sitemap/_default" title="Default" view="#main-view" panel-close></f7-list-item>
        <f7-list-item link="/sitemap/other" title="Other sitemap" view="#main-view" panel-close></f7-list-item>
        <f7-list-item link="/sitemap/floors" title="By Floor" view="#main-view" panel-close></f7-list-item>
        <f7-list-item link="/sitemap/devices" title="By Device" view="#main-view" panel-close></f7-list-item> -->
      </f7-list>      
      <f7-block-title>Administration</f7-block-title>
      <f7-list>
        <f7-list-item link="/settings/" title="Settings" view="#main-view" panel-close>
          <f7-icon slot="media" ios="f7:gears" md="material:settings"></f7-icon>
        </f7-list-item>
        <f7-list-item link="/about/" title="Help &amp; About" view="#main-view" panel-close>
          <f7-icon slot="media" ios="f7:help_round" md="material:help"></f7-icon>
        </f7-list-item>
      </f7-list>
    </f7-panel>

    <!-- Right Panel -->
    <f7-panel right reveal theme-dark>
      <panel-right />
      <!-- <f7-view url="/panel-right/"></f7-view> -->
    </f7-panel>

    <!-- Main View -->
    <f7-view id="main-view" url="/" main></f7-view>

    <!-- Popup -->
    <f7-popup id="popup">
      <f7-view>
        <f7-page>
          <f7-navbar title="Popup" back-link="Back">
            <f7-nav-right>
              <f7-link popup-close>Close</f7-link>
            </f7-nav-right>
          </f7-navbar>
          <f7-block>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Neque, architecto. Cupiditate laudantium rem nesciunt numquam, ipsam. Voluptates omnis, a inventore atque ratione aliquam. Omnis iusto nemo quos ullam obcaecati, quod.</f7-block>
        </f7-page>
      </f7-view>
    </f7-popup>

    <!-- Login Screen -->
    <setup-wizard />

  </f7-app>
</template>

<style lang="stylus" scoped>
.panel-left
  .logo
    padding 3rem 2rem
    background-color #fff
    height 50px
  .list
    margin-top 0
</style>

<script>
// Import Routes
import routes from './routes.js'
import PanelRight from './pages/panel-right.vue'
import SetupWizard from './pages/wizards/setup-wizard.vue'

export default {
  components: {
    PanelRight,
    SetupWizard
  },
  data () {
    return {
      sitemaps: [],
      // Framework7 parameters here
      f7params: {
        id: 'io.framework7.testapp', // App bundle ID
        name: 'Framework7', // App name
        theme: (document.documentURI && document.documentURI.indexOf('?theme=ios') > 0) ? 'ios' :
               (document.documentURI && document.documentURI.indexOf('?theme=md') > 0) ? 'md' :
               'auto', // Automatic theme detection
        // App routes
        routes: routes,
        view: {
          pushState: true,
        },
        panel: {
          swipe: 'left',
          leftBreakpoint: 768,
          // rightBreakpoint: 1440,
        }
      },
    }
  },
  created () {
    fetch('/rest/sitemaps').then((resp) => {
      const json = resp.json()
      json.then((j) => {
        this.sitemaps = j
      })
    })
  }
}
</script>
