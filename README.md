![](https://img.shields.io/badge/version-0.3.1-green.svg)
![](https://img.shields.io/badge/status-beta-red.svg)
[![](https://data.jsdelivr.com/v1/package/gh/Lextenso/IBM-Watson-Algolia-connector-instantsearch.js/badge)](https://www.jsdelivr.com/package/gh/Lextenso/IBM-Watson-Algolia-connector-instantsearch.js)
# IBM Watson connector for Algolia InstantSearch.js
IBM Watson connector for Algolia InstantSearch.js allows you to easily add Speech to Text (STT) as a widget to your Algolia search.

![Algolia and watson = love](https://content.lext.so/github/IBM-watson-algolia-connector-instantsearch.js/assets/algolia-waston-love.png)

This repo is initially developed by [Lextenso](https://www.lextenso.fr), open to the community :hearts: and isn't affiliated or supported by IBM or Algolia.

__PLEASE NOTE__ : This current version `0.3.1` still in beta, please use carefully.

## Built with IBM Watson Speech Services
This is a middleware between _Algolia InstantSearch.js_ and _IBM Watson_.
This Algolia connector is bundled with the NPM package `watson-speech/speech-to-text/recognize-microphone` and `instantsearch.js/es/connectors`.

The connector handle :
* Microphone browser authorization
* Stream voice to Watson (via WebSocket)
* Output text results in search input
* Refine Algolia search with new values in realtime

#### Requirements
This connector doesn't inject any templates, you have to develop your own HTML element to trigger Watson with this connector.

You also need [Algolia](https://www.algolia.com/users/sign_up) and [IBM Watson](https://www.ibm.com/watson/services/speech-to-text/) credentials. (_Elementary, my dear Watson_)

## Dependencies
* [instantsearch.js](https://github.com/algolia/instantsearch.js/) : 2.10.0
* [watson-speech](https://github.com/watson-developer-cloud/speech-javascript-sdk/) : 0.34.2

## Installation

### Standalone
Pre-compiled bundle is available on [jsdelivr.com CDN](https://www.jsdelivr.com/package/gh/Lextenso/IBM-Watson-Algolia-connector-instantsearch.js) :
 ```html
<script src="https://cdn.jsdelivr.net/gh/Lextenso/IBM-Watson-Algolia-connector-instantsearch.js@0.3.1-beta/dist/IBMWatsonAlgoliaConnector.js" integrity="sha256-AYjIPJf0idsNjSmQuUU1vhqBzESeV7rXS8uIvjFwXIw=" crossorigin="anonymous"></script>
```

Or build the connector with Webpack :
```console
sher-lock:MyProject$ npm run build
```

### ES Module with Webpack
Import the IBMWatsonAlgoliaConnector module which is available in the `/src` directory.

```js
import IBMWatsonAlgoliaConnector from './IBMWatsonAlgoliaConnector';
```

## Configuration
All attributes are listed and detailed in the [specifications section](#specifications).

#### Standalone
```html
<script src="https://cdn.jsdelivr.net/gh/Lextenso/IBM-Watson-Algolia-connector-instantsearch.js@0.3.1-beta/dist/IBMWatsonAlgoliaConnector.js" integrity="sha256-AYjIPJf0idsNjSmQuUU1vhqBzESeV7rXS8uIvjFwXIw=" crossorigin="anonymous"></script>
<script type="text/javascript">
const search = instantsearch(config);

// [...]

search.addWidget(
    IBMWatsonAlgoliaConnector.default({
        container: {
            searchInput: [string],
            voiceButton: [string]
        },
        template: {
            onActiveClass: [string],
            onInactiveClass: [string],
            // OR
            onStateChange: [function (state => 'active' || 'inactive')]
        },
        watsonConfig: {
            tokenURL: [string],
            model: [string],
            continuous: [boolean]
        }
    })
);

// [...]

search.start();
</script>
```

#### NPM - Webpack
```js
import InstantSearch from 'instantsearch.js/es/';
import IBMWatsonAlgoliaConnector from './IBMWatsonAlgoliaConnector';

const search = InstantSearch(config);

// [...]

search.addWidget(
    IBMWatsonAlgoliaConnector({
        container: {
            searchInput: [string],
            voiceButton: [string],
        },
        template: {
            onActiveClass: [string],
            onInactiveClass: [string],
            // OR
            onStateChange: [function (state => 'active' || 'inactive')]
        },
        watsonConfig: {
            tokenURL: [string],
            model: [string],
            continuous: [boolean]
        }
    })
);

// [...]

search.start();
```
## Build examples

Two examples are available : standalone and ES Module.
Each example require a server-side endpoint to fetch Watson's token, you can find examples [here](https://github.com/watson-developer-cloud/speech-javascript-sdk/tree/master/examples)

### Standalone

Change the API endpoint to fetch Watson's token in the `example/standalone/app.js` file.
```js
search.addWidget(
    IBMWatsonAlgoliaConnector.default({
        // [...]
        watsonConfig: {
            tokenURL: 'https://local.example.dev/api/watson/get-token',
            // [...]
        }
    })
);
```
Then
```console
sher-lock:IBM-watson-algolia-connector-instantsearch.js$ npm run build-example
```

### ES Module with Webpack
Change the API endpoint to fetch Watson's token in the `example/es/src/main.js` file.
```js
search.addWidget(
    IBMWatsonAlgoliaConnector({
        // [...]
        watsonConfig: {
            tokenURL: 'https://local.example.dev/api/watson/get-token',
            // [...]
        }
    })
);
```
Then
```console
sher-lock:IBM-watson-algolia-connector-instantsearch.js$ npm run build-example
```

## Specifications
<table>
    <tr>
        <th>Attributes</th>
        <th></th>
        <th></th>
        <th>Required ?</th>
        <th>Comment</th>
    </tr>
    <tr>
        <td>container</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>searchInput</td>
        <td>[string]</td>
        <td>Yes</td>
        <td>This string must be a <a href="https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector">valid CSS selector</a> string.</td>
    </tr>
    <tr>
        <td></td>
        <td>voiceButton</td>
        <td>[string]</td>
        <td>Yes</td>
        <td>This string must be a <a href="https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector">valid CSS selector</a> string.</td>
    </tr>
    <tr>
        <td>template</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>onActiveClass</td>
        <td>[string]</td>
        <td>Yes (if NOT `onStateChange`)</td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>onInactiveClass</td>
        <td>[string]</td>
        <td>Yes (if NOT `onStateChange`)</td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td>onStateChange</td>
        <td>[function (state =&gt; active &vert;&vert;&nbsp;inactive)]</td>
        <td>Yes (Only if `onInactiveClass` and `onActiveClass` are not enouth for your needs)</td>
        <td>This callback will be triggered every time the state of Watson change.<br/>
        Possible values: active OR inactive<br><strong>NOTE</strong> : The callback function override `onInactiveClass` and `onActiveClass`</td>
    </tr>
    <tr>
        <td>watsonConfig</td>
        <td></td>
        <td></td>
        <td></td>
        <td>Watson configuration (<a href="http://watson-developer-cloud.github.io/speech-javascript-sdk/master/module-watson-speech_speech-to-text_recognize-microphone.html">see doc</a>)<br><strong>NOTE</strong> : 'token' and 'outputElement' will be overridden on connector start</td>
    </tr>
    <tr>
        <td></td>
        <td>tokenURL</td>
        <td>[string]</td>
        <td>Yes</td>
        <td>Must be an URL to a back-end service</td>
    </tr>
    <tr>
        <td></td>
        <td>model</td>
        <td>[string]</td>
        <td>No</td>
        <td>Check possible values <a href="https://www.ibm.com/watson/developercloud/speech-to-text/api/v1/node.html?node#get-model">here</a></td>
    </tr>
    <tr>
        <td></td>
        <td>continuous</td>
        <td>[boolean]</td>
        <td>No</td>
        <td>Watson stop listening when it detects final sentence</td>
    </tr>
</table>

## Todo

* NPM publish
* On start state (default state in case of unsupported browsers)
* Advanced fetch Watson token function (REST API, Auth, etc ...)
* Better cross-browser testing
* Improve documentation

## Author
* Julien Madiot ([@Madi-Ji](https://github.com/Madi-Ji))

---
IBM®, IBM Watson® and Watson™ are United States trademarks owned by International Business Machines Corporation ("IBM") and might also be trademarks or registered trademarks in other countries. ([Copyright and trademark information](https://www.ibm.com/legal/us/en/copytrade.shtml))

Algolia and all other trademarks, service marks, graphics and logos used are trademarks or registered trademarks of Algolia or Algolia’s licensors. ([Acceptable use policy](https://www.algolia.com/policies/acceptable-use))
