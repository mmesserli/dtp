# iFrame embedding of identification

The identification process supports to run as an embedded element on your web application. The identification link will be embedded as an `iframe` in the external application, an can be run fully integrated in the external application.

#### Embedding the identification

**For static HTML**

You can import the package using CDN. Add the following script tag to your HTML file

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>m_IDeal Embed Example</title>
</head>
<body>
    <div id="mideal-container"></div>
    <script type="module">
        import MidealEmbed from 'https://cdn.jsdelivr.net/npm/@mesoneer-ag/mideal-embed'
        // Initialize the m_IDeal embed
        const mideal = new MidealEmbed();
        mideal.start({
          startUrl: startUrl,
          enclosingDomElement: iframeContainer,
          style: {
            width: '100%',
            height: '860px',
          },
          sandbox: 'allow-scripts allow-same-origin',
          onMessage: handleReceivedMessage,
        });
    </script>
</body>
</html>
```

**For typescript projects (Angular, React...)**

1. Install the package: First, install the package using npm:

```bash
npm install @mesoneer-ag/mideal-embed
```

2. Import and Use the package: In your component, you can import and use the package as follows:

```typescript
import { Component, OnInit } from '@angular/core';
import MidealEmbed from '@mesoneer-ag/mideal-embed';

@Component({
  selector: 'app-mideal',
  template: '<div id="mideal-container"></div>',
  styleUrls: ['./mideal.component.css']
})
export class MidealComponent implements OnInit {

  ngOnInit(): void {
    const mideal = new MidealEmbed();
    mideal.start({
      startUrl: startUrl,
      enclosingDomElement: iframeContainer,
      style: {
        width: '100%',
        height: '860px',
      },
      sandbox: 'allow-scripts allow-same-origin',
      onMessage: handleReceivedMessage,
    });
  }
}
```

3. Add the Component to Your Template: Finally, include the component in your Angular template where you want the application embed to appear:

```html
<app-mideal></app-mideal>
```

**NOTE**: Following permissions need to be allowed on iframe which will be added automatically by the SDK

* camera
* microphone
* fullscreen

**API Reference**

`start` start loading the application into an iframe. It takes four parameters.

* `startUrl (Mandatory)` String - the URL could be the providers Standalone URL, a Scheduled Case URL or a Signing URL.
* `enclosingDomElement (Mandatory)` Element - DOM element that the iframe of the application will be loaded into it.
* `style (Optional)` Object - The object will be used to apply CSS style into the iframe element. Currently, only two options are supported: width and height. The values can be any valid CSS value.
* `sandbox` Function - The value of iframe sandbox attribute to specify a set of extra restrictions for the content in the iframe. The value 'allow-scripts allow-same-origin' is required if use this sandbox.
* `onMessage (Optional)` Function - the callback function to handle received events from the providers application iframe. It uses browser's window postMessage API to inform the parent frame when the process ends.

```js
import MidealEmbed from '@mesoneer-ag/mideal-embed';

const mideal = new MidealEmbed();
mideal.start({startUrl, enclosingDomElement, style, sandbox, onMessage});
```

**Example:**

```js
// Suppose this is a EventListener for a click on a button labeled "Register".
// Your web application's layout has already defined a DOM element to display the `iframe`,
// whose `id` is defined as `mideal-enclosing-container`.
function onRegisterButtonClicked(event) {
    const mideal = new MidealEmbed();
    mideal.start({
        startUrl: "https://autoid.id-validation.ch/scan?scheduled-case-id=xxxyyy&language=de",
        enclosingDomElement: document.getElementById("mideal-enclosing-container"),
        style: {
            width: "100%",
            height: "860px"
        },
        sandbox: "allow-scripts allow-same-origin",
        /*
        * The following is the schema declaration of the midealEvent message object.
        * midealEvent:
        *   type: object
        *   properties:
        *     scanResult:
        *       description: |
        *         Identification scan result from embedded application
        *         * `IDENTIFICATION_SUCCESSFUL` - result of identification was successful
        *         * `IDENTIFICATION_FAILED` - identification failed
        *         * `TIMEOUT` - identification session has been time out
        *       type: string
        *       nullable: false
        *     date:
        *       description: Message timestamp in milliseconds elapsed since the epoch
        *       type: integer
        *       nullable: false
        *     statusReason:
        *       description: The reason that the current scanResult value is updated to.
        *       type: string
        *       nullable: true
        *     statusDetails:
        *       description: The details of reason that the current scanResult value is updated to.
        *       type: string
        *       nullable: true
        *
        * Example:
        * midealEvent = {
        *   scanResult: "IDENTIFICATION_FAILED",
        *   date: 1690424149263,
        *   statusReason: "ABORTED",
        *   statusDetails: "STALLED_TIMEOUT"
        * }
        **/
        onMessage: function (midealEvent) {
            /*
            * Note: The midealEvent fields can be omitted, for example, 
            * if your application does not use signing process
            * the signingResult and signingErrors should be ignored.
            **/
            const {scanResult, date} = midealEvent;
            if (scanResult === "IDENTIFICATION_SUCCESSFUL") {
                alert("Thanks for complating the process");
            }
        }
    });
}
```

Added `iframe` sample:

```
<div id="mideal-enclosing-container">
  <iframe id="mideal-embedded" allow="camera;microphone" allowfullscreen="true" allowtransparency="true" sandbox="allow-scripts allow-same-origin" src="https://autoid.id-validation.ch/scan?tenantId=xxxyyy&language=de" width="100%" height="860px" frameborder="0">...</iframe>
</div>
```

**IMPORTANT**: `frame-src` directive needs to be added to the `Content-Security-Policy` (CSP) of the external application which embeds our application.

```
Content-Security-Policy: frame-src <embedded-source> blob:;
```

For example, the external application will embed our application using `SANDBOX` environment

```
Content-Security-Policy: frame-src https://sandbox-autoid.id-validation.ch/ blob:;
```
