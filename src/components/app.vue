<template>
<f7-app :params="f7params">
  <!-- Status bar overlay for fullscreen mode-->
  <f7-statusbar></f7-statusbar>

  <!-- Left panel with cover effect-->
  <f7-panel left cover theme-dark>
    <f7-view>
      <f7-page>
        <f7-navbar title="Left Panel"></f7-navbar>
        <f7-block>Left panel content goes here</f7-block>
      </f7-page>
    </f7-view>
  </f7-panel>


  <!-- Right panel with reveal effect-->
  <f7-panel right reveal theme-dark>
    <f7-view>
      <f7-page>
        <f7-navbar title="Right Panel"></f7-navbar>
        <f7-block>Right panel content goes here</f7-block>
      </f7-page>
    </f7-view>
  </f7-panel>


  <!-- Your main view, should have "view-main" class -->
  <f7-view main class="safe-areas" url="/"></f7-view>


  <!-- Popup -->
  <f7-popup id="my-popup">
    <f7-view>
      <f7-page>
        <f7-navbar title="cordova-plugin-geolocation">
          <f7-nav-right>
            <f7-link popup-close>Close</f7-link>
          </f7-nav-right>
        </f7-navbar>
        <f7-block>
          <p>watchID={{watchID}}</p>
          <f7-block>
            <f7-list inline-labels no-hairlines-md>
              <f7-list-input
                      label="maximumAge"
                      type="number"
                      placeholder="maximumAge"
                      id="maximumAge-input"
                      :value="maximumAge"
                      @input="maximumAge = $event.target.value"
              >
              </f7-list-input>
              <f7-list-input
                      label="timeout"
                      type="number"
                      placeholder="timeout"
                      id="timeout-input"
                      :value="timeout"
                      @input="timeout = $event.target.value"
              >
              </f7-list-input>
            </f7-list>
          </f7-block>
          <f7-block>
            <f7-row>
              <f7-col>
                <f7-button fill color="green" @click="watch">watchPosition</f7-button>
              </f7-col>
            </f7-row>
            <f7-row style="margin-top: 5px;">
              <f7-col>
                <f7-button fill @click="currentPosition">getCurrentPosition</f7-button>
              </f7-col>
              <f7-col>
                <f7-button fill color="red" @click="clear">clear watchID</f7-button>
              </f7-col>
            </f7-row>
            <f7-row style="margin-top: 5px;">
              <f7-col>
                <f7-button fill width="60" @click="clearDisplay">clear display:{{geoLogArray.length}}</f7-button>
              </f7-col>
            </f7-row>
          </f7-block>
          <f7-block strong>
            <f7-row v-for="(item , index ) in geoLogArray" :key="item">
              <f7-row>{{item}}</f7-row>
            </f7-row>
          </f7-block>
        </f7-block>
      </f7-page>
    </f7-view>
  </f7-popup>

  <!-- Popup -->
  <f7-popup id="my-popup-1">
    <f7-view>
      <f7-page>
        <f7-navbar title="cordova-background-geolocation-lt">
          <f7-nav-right>
            <f7-link popup-close>Close</f7-link>
          </f7-nav-right>
        </f7-navbar>
        <f7-block>
          <f7-block>
            <f7-row>
              <f7-col>
                <f7-button fill @click="ready">ready</f7-button>
              </f7-col>
              <f7-col>
                <f7-button fill @click="start">start</f7-button>
              </f7-col>
            </f7-row>
            <f7-block>
              <f7-row>
                <f7-col>
                  <f7-button fill @click="getCurrent">getCurrentPosition</f7-button>
                </f7-col>
              </f7-row>
            </f7-block>
            <f7-block>
              <f7-row>
                <f7-col>
                  <f7-button fill color="green" @click="clearBgDisplay">clear display:{{bgGeoLogArray.length}}</f7-button>
                </f7-col>
              </f7-row>
            </f7-block>
          </f7-block>
          <f7-block strong>
            <f7-row v-for="(item , index ) in bgGeoLogArray" :key="item">
              <f7-row>{{item}}</f7-row>
            </f7-row>
          </f7-block>
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
          <f7-list-button title="Sign In" login-screen-close @click="alertLoginData"></f7-list-button>
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
  import cordovaApp from '../js/cordova-app.js';
  import routes from '../js/routes.js';

  var bgGeo = {};

  export default {
    data() {
      return {
        // Framework7 Parameters
        f7params: {
          id: 'io.framework7.BackgroundGeolocationDemo', // App bundle ID
          name: 'BackgroundGeolocationDemo', // App name
          theme: 'auto', // Automatic theme detection
          // App root data
          data: function () {
            return {
              user: {
                firstName: 'John',
                lastName: 'Doe',
              }
            };
          },

          // App routes
          routes: routes,



          // Input settings
          input: {
            scrollIntoViewOnFocus: !!this.$device.cordova,
            scrollIntoViewCentered: !!this.$device.cordova,
          },
          // Cordova Statusbar settings
          statusbar: {
            overlay: this.$device.cordova && this.$device.ios || 'auto',
            iosOverlaysWebView: true,
            androidOverlaysWebView: false,
          },
        },

        // Login screen data
        username: '',
        password: '',
        watchID: '',
        geoLogArray: [],
        bgGeoLogArray: [],
        maximumAge: 300000,
        timeout: 30000,
        enableHighAccuracy: true
      }
    },
    computed: {

    },
    methods: {
      alertLoginData() {
        this.$f7.dialog.alert('Username: ' + this.username + '<br>Password: ' + this.password);
      },
      clearDisplay(){
        this.geoLogArray = [];
      },
      clearBgDisplay(){
        this.bgGeoLogArray = [];
      },
      onSuccess(position) {
        this.$f7.preloader.hide();
        console.log('onSuccess:', position);
        this.geoLogArray.push(position.timestamp + ':(' + position.coords.latitude + ',' + position.coords.longitude + ')');
      },
      onSuccessAlert(position) {
        this.$f7.preloader.hide();
        console.log('getCurrentPosition:', position);
        this.geoLogArray.push('getCurrentPosition:' + position.coords.latitude + ',' + position.coords.longitude);
        alert(position.coords.latitude + ',' + position.coords.longitude);
      },
      onError(error) {
        this.$f7.preloader.hide();
        console.log('onError:', error);
        this.geoLogArray.push('onError:' + error.code + ':' + error.message);
      },
      watch() {
        this.$f7.preloader.show();
        this.watchID = navigator.geolocation.watchPosition(this.onSuccess, this.onError, { maximumAge: this.maximumAge, timeout: this.timeout, enableHighAccuracy: this.enableHighAccuracy });
        this.geoLogArray.push('watchPosition:' + ':(maximumAge=' + this.maximumAge + ',timeout=' + this.timeout + ',enableHighAccuracy=' + this.enableHighAccuracy + ')');
        console.log('watchPosition:' + ':(maximumAge=' + this.maximumAge + ',timeout=' + this.timeout + ')');
        console.log('watchID:' + this.watchID);
      },
      clear(){
        this.geoLogArray = [];
        navigator.geolocation.clearWatch(this.watchID);
        this.watchID = '';
      },
      currentPosition(){
        this.$f7.preloader.show();
        navigator.geolocation.getCurrentPosition(this.onSuccessAlert, this.onError, { enableHighAccuracy: true });
      },
      ready(){
        this.$f7.preloader.show();
        // 1.  Listen to events
        bgGeo = window.BackgroundGeolocation;

        bgGeo.onLocation((location) => {
          console.log('[location] -', location);
          this.bgGeoLogArray.push('[location] -' + location.timestamp + ':' + location.coords.latitude + ',' + location.coords.longitude);
          // alert('latitude:' + location.coords.latitude + '\n' + 'longitude:' + location.coords.longitude);
        });

        bgGeo.onMotionChange((event) => {
          console.log('[motionchange] -', event.isMoving, event.location);
          this.bgGeoLogArray.push('[motionchange] -' + 'event.isMoving=' + event.isMoving + ':' + event.location.coords.latitude + ',' + event.location.coords.longitude);
          // alert('event:' + event);
        });

        bgGeo.onHttp((response) => {
          console.log('[http] - ', response.success, response.status, response.responseText);
        });

        bgGeo.onProviderChange((event) => {
          console.log('[providerchange] -', event.status, event.enabled, event.gps, event.network);
          this.bgGeoLogArray.push('[providerchange]:\nstatus:' + event.status + '\n' + 'enabled:' + event.enabled + '\n' + 'gps:' + event.gps + '\n' + 'network:' + event.network + '\n');
          // alert('[providerchange]:\nstatus:' + event.status + '\n' + 'enabled:' + event.enabled + '\n' + 'gps:' + event.gps + '\n' + 'network:' + event.network + '\n');
        });
        // 2. Execute #ready method:
        let self = this;
        bgGeo.ready({
          reset: true,
          debug: true,
          logLevel: bgGeo.LOG_LEVEL_VERBOSE,
          desiredAccuracy: bgGeo.DESIRED_ACCURACY_HIGH,
          distanceFilter: 10,
          url: 'http://my.server.com/locations',
          autoSync: true,
          stopOnTerminate: false,
          startOnBoot: true
        }, function(state) {    // <-- Current state provided to #configure callback
          // 3.  Start tracking
          console.log('BackgroundGeolocation is configured and ready to use');
          self.bgGeoLogArray.push('BackgroundGeolocation is configured and ready to use');
          if (!state.enabled) {
            bgGeo.start().then(function() {
              console.log('- BackgroundGeolocation tracking started');
              self.bgGeoLogArray.push('- BackgroundGeolocation tracking started');
            });
          }
        });
        this.$f7.preloader.hide();
      },
      start(){
        this.$f7.preloader.show();
        bgGeo.start().then((data)=>{
          this.$f7.preloader.hide();
          console.log(data);
          this.bgGeoLogArray.push('start success:', data);
          alert(data);
        }).catch((err) => {
          this.$f7.preloader.hide();
          console.log(err);
          this.bgGeoLogArray.push('start error:', err);
          alert(err);
        });
      },
      getCurrent(){
        this.$f7.preloader.show();
        bgGeo.getCurrentPosition().then((data)=>{
          this.$f7.preloader.hide();
          console.log(data);
          this.bgGeoLogArray.push('getCurrentPosition success:', data);
          alert(data);
        }).catch((err) => {
          this.$f7.preloader.hide();
          console.log(err);
          this.bgGeoLogArray.push('getCurrentPosition error:', err);
          alert(err);
        });
      }
    },
    mounted() {
      this.$f7ready((f7) => {
        // Init cordova APIs (see cordova-app.js)
        if (f7.device.cordova) {
          cordovaApp.init(f7);
        }
        // Call F7 APIs here
      });
    },
    created() {

    }
  }
</script>