<template>
  <div>

    <head>
      <title>Vue QR-Code Reader DEMO</title>

      <meta charset="utf-8">
      <meta
        name="viewport"
        content="width=device-width, initial-scale=1, shrink-to-fit=no"
      >

      <script src="https://unpkg.com/vue@2.6.10/dist/vue.min.js"></script>
      <script src="https://unpkg.com/vue-qrcode-reader@2.0.3/dist/vue-qrcode-reader.browser.js"></script>
      <link
        rel="stylesheet"
        href="https://unpkg.com/vue-qrcode-reader@2.0.3/dist/vue-qrcode-reader.css"
      >
    </head>

    <body>
      <div id="app">
        <p>
          Last result: <b>{{ decodedContent }}</b>
        </p>

        <p class="error">
          {{ errorMessage }}
        </p>

        <qrcode-stream
          @decode="onDecode"
          @init="onInit"
        ></qrcode-stream>
      </div>
    </body>
  </div>
</template>
<script>
export default {
  data() {
    return {
      decodedContent: "",
      errorMessage: ""
    };
  },

  methods: {
    onDecode(content) {
      this.decodedContent = content;
    },

    onInit(promise) {
      promise
        .then(() => {
          console.log("Successfully initilized! Ready for scanning now!");
        })
        .catch(error => {
          if (error.name === "NotAllowedError") {
            this.errorMessage = "Hey! I need access to your camera";
          } else if (error.name === "NotFoundError") {
            this.errorMessage = "Do you even have a camera on your device?";
          } else if (error.name === "NotSupportedError") {
            this.errorMessage =
              "Seems like this page is served in non-secure context (HTTPS, localhost or file://)";
          } else if (error.name === "NotReadableError") {
            this.errorMessage =
              "Couldn't access your camera. Is it already in use?";
          } else if (error.name === "OverconstrainedError") {
            this.errorMessage =
              "Constraints don't match any installed camera. Did you asked for the front camera although there is none?";
          } else {
            this.errorMessage = "UNKNOWN ERROR: " + error.message;
          }
        });
    }
  }
};
</script>
