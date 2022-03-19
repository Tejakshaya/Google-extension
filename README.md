# Google-extension
a google extension which is made by jeffswartz and his team.
This extension allows you to use the screen-sharing support in older versions of Chrome and Opera with the OpenTok.js client library.

Important: Chrome 72+ and Opera 59+ no longer require an extension for screen sharing. The browser prompts the end user for access to the screen, as it would for access to the camera. The extension in this repo is only included to support older versions of Chrome and Opera.

This is a variation of the extension created by Muaz Khan with some tweaks to make it more suitable for use with the OpenTok API.

This is version 2 of the extension. With version 2, the client can use the extension immediately after installing it using the Inline Installation method, without reloading the page. When calling the OT.registerScreenSharingExtension() method (see below), you must now pass in the version number, 2.

(Important: As of June 12, 2018, inline installation is deprecated.)

Customizing the extension for your website
Fork (or simply download) this github repo.

Edit the contents of the manifest.json file. Be sure to change the name and author settings, and ensure that matches is set to your own domains only. You will also want to replace the icon files (logo16.png, logo48.png, logo128.png, and logo128transparent.png) with your own website logos. You should change the version setting with each new version of your extension. And you may want to change the description.

For more information, see the Chrome extension manifest documentation.

See the following sections for testing and deploying your extension.

Testing your unpacked extension
Open chrome://extensions and drag the ScreenSharing directory onto the page, or click 'Load unpacked extension...'. For more information see Chrome's documentation on loading unpacked extensions.

Locate the screensharing-test.html file in the root of this repo. Copy the file to a web server. (Screen-sharing access requires that the file be installed on a web server. You cannot load the file from a file: URL.)

Edit the following values in the file:

apiKey -- Set this to your OpenTok API key. See https://dashboard.tokbox.com
sessionId -- An OpenTok session ID
token -- A valid token for the OpenTok session
extensionId -- The ID of your screen-sharing extension. You can get the ID of the extension in the chrome://extensions page after enabling Developer mode in the top-right. (It looks like ffngmcfincpecmcgfdpacbdbdlfeeokh.)
In Chrome, navigate to the screensharing-test.html URL on your server. Be sure to load the page via HTTPS. (Screen-sharing requires HTTPS.)

Upon connecting to the OpenTok session, the app publishes a stream using the camera as the video source. Click the Share your screen button to publish a screen-sharing stream.

Open a new Chrome window or tab and navigate to the HTTPS screensharing-test.html URL.

Upon connecting to the OpenTok session, the app publishes a stream using the camera as the video source. It also subscribes to the two streams published by the other page (the camera stream and the screen-sharing stream).

Packaging and deploying your extension for use at your website
For your production website, you need to package your screen-sharing extension and register it in the Chrome Web Store.

See the Chrome documentation on publishing your app for details on publishing your extension in the Chrome Web Store.

You can use the Chrome inline installation to initiate installation of your extension "inline" from your site. (Important: As of June 12, 2018, inline installation is deprecated.)

In your app, you need to register the ID of the extension using the OT.registerScreenSharingExtension() method (defined in the OpenTok.js library). (See the next section and the code in the screensharing-test.html file in this repo.)

How to use with OpenTok.js?
After installing the extension (either locally, or after publishing through the Chrome Web Store), get the extension ID from chrome://extensions (it looks like ffngmcfincpecmcgfdpacbdbdlfeeokh).

<script src="//static.opentok.com/v2/js/opentok.min.js"></script>
Register you extension:

OT.registerScreenSharingExtension('chrome', 'YOUR-EXTENSION-ID', 2);
To test if the extension is available you can use the OT.checkScreenSharingCapability() method:

OT.checkScreenSharingCapability(function(response) {
  if (!response.supported || response.extensionRegistered === false) {
    // This browser does not support screen sharing
  } else if (response.extensionInstalled === false) {
    // prompt to install the response.extensionRequired extension
  } else {
    // Screen sharing is available
  }
});
If you are using Inline Installation their are additional APIs you can use. (Important: As of June 12, 2018, inline installation is deprecated.)

To publish a screen:

var el = document.createElement('div');
document.body.appendChild(el);
var pub = OT.initPublisher(el, {
  name: 'Screen',
  mirror: false,
  audioSource: null,
  videoSource: 'screen',
  maxResolution: { width: 1280, height: 720 }, // max resolution to encode screen in
  width: 1280, // width of preview
  height: 720, // height of preview
}, function(error) {
  if (error) {
    // handle the error
  }
  // resize the publisher preview to match the encoded video
  pub.element.style.width = pub.videoWidth() + 'px';
  pub.element.style.height = pub.videoHeight() + 'px';
})
Browser Support
Google Chrome (version 34+) only.

More information
See the OpenTok.js screen-sharing documentation.
and also I kinda modified it....
