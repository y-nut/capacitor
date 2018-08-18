*Capacitor is currently in beta and is not production ready. We'd love your help testing!*

# ⚡️ Customization of Capacitor
I had a problem with the Proxy when I tried to run my Capacitor application in IE11. 
It is because Capacitor is not supporting IE11 but there is no stop in the code execution to avoid throwing an error if the client
is indeed using IE11.

I have made a simple if-then-else statement to ensure that the Plugins doesn't break the app if run in IE11. I have yet to test other IE and Edge versions.

In particular I have edited the file


"ng 6 project"/node_modules/@capacitor/core/dist/esm/web-runtime.js.
The npm version is not similar to this repo so I have just copied the file below:
I am using ngx-device-detector to know whether to call Plugins or not.

var CapacitorWeb = /** @class */ (function() {
    function CapacitorWeb() {
      var _this = this;
      this.Plugins = {};
      this.platform = 'web';
      this.isNative = false;
  
      /*! Capacitor: https://ionic-team.github.io/capacitor/ - MIT License */
  
      // Opera 8.0+
      var isOpera = (!!window.opr && !!opr.addons) || !!window.opera || navigator.userAgent.indexOf(' OPR/') >= 0;
  
      // Firefox 1.0+
      var isFirefox = typeof InstallTrigger !== 'undefined';
  
      // Safari 3.0+ "[object HTMLElementConstructor]" 
      var isSafari = /constructor/i.test(window.HTMLElement) || (function(p) {
        return p.toString() === "[object SafariRemoteNotification]";
      })(!window['safari'] || (typeof safari !== 'undefined' && safari.pushNotification));
  
      // Internet Explorer 6-11
      var isIE = /*@cc_on!@*/ false || !!document.documentMode;
  
      // Edge 20+
      var isEdge = !isIE && !!window.StyleMedia;
  
      // Chrome 1+
      var isChrome = !!window.chrome && !!window.chrome.webstore;
  
      // Blink engine detection
      var isBlink = (isChrome || isOpera) && !!window.CSS;
  
      if (isChrome || isFirefox) {
        console.log('isvalid')
        // Build a proxy for the Plugins object that returns the "Noop Plugin"
        // if a plugin isn't available
        this.Plugins = new Proxy(this.Plugins, {
          get: function(target, prop) {
            if (typeof target[prop] === 'undefined') {
              var thisRef_1 = _this;
              return new Proxy({}, {
                get: function(_target, _prop) {
                  if (typeof _target[_prop] === 'undefined') {
                    return thisRef_1.pluginMethodNoop.bind(thisRef_1, _target, _prop, prop);
                  } else {
                    return _target[_prop];
                  }
                }
              });
            } else {
              return target[prop];
            }
          }
        });
  
        CapacitorWeb.prototype.pluginMethodNoop = function(_target, _prop, pluginName) {
          return Promise.reject(pluginName + " does not have web implementation.");
        };
        CapacitorWeb.prototype.getPlatform = function() {
          return this.platform;
        };
        CapacitorWeb.prototype.isPluginAvailable = function(name) {
          return this.Plugins.hasOwnProperty(name);
        };
        CapacitorWeb.prototype.handleError = function(e) {
          console.error(e);
        };
  
      } else {
        console.log('no valid')
      }
  
    }
    return CapacitorWeb;
  }());
  export {
    CapacitorWeb
  };
  //# sourceMappingURL=web-runtime.js.map
  

# ⚡️ Create cross-platform apps with JavaScript and the Web ⚡️

Capacitor is a cross-platform API and code execution layer that makes it easy to call Native SDKs from web code and to write custom Native plugins that your app might need.  Additionally, Capacitor provides first-class Progressive Web App support so you can write one app and deploy it to the app stores, _and_ the mobile web.

Capacitor is being designed by the Ionic Framework team as an eventual alternative to Cordova, though backwards compatibility with Cordova plugins is a priority and is actively being worked on. Capacitor can be used without Ionic Framework, but soon it'll become a core part of the Ionic developer experience.

Capacitor also comes with a Plugin API for building native plugins. On iOS, first-class Swift support is available, and much of the iOS Capacitor runtime is written in Swift. Plugins may also be written in Objective-C. On Android, support for writing plugins with Java and Kotlin is supported.
 
## Roadmap

_Disclaimer: Our roadmap is subject to change at any time and has no specific date guarantees_

2018

 - __Cordova Plugin Integration__
   - Preliminary support for using plugins from the existing Cordova community
 - __Native Shell Add-ons__
   - Support for interacting with Native UI shell elements, such as native menus, tabs, and navigation, with 1-1 fallbacks to the web for first-class Progressive Web App and Electron support.
 - __Electron support__
   - Support for building Electron apps and interacting with Node.js libraries
 - __Enterprise Premium Plugins__
   - Paid add-on plugins with support for common Enterprise use cases, such as storage, authentication, security, and more
   - Developer Support options with SLAs and priority patches
   - We are working with a few large teams/businesses as early development partners. Interested? Email [max@ionicframework.com](mailto:max@ionicframework.com)

## Contributing

Contributing to Capacitor may involve writing TypeScript, Swift/Objective-C, Java, or Markdown depending on the component you are working on. We are looking for help in any of these areas!

Please read the [Contributing](.github/CONTRIBUTING.md) guide for more information.
