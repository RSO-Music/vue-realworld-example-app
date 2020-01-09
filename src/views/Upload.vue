<template>
  <div v-on:click="upload();">dfgdfgdfg</div>
</template>

<script>
// const request = require('request');
import request from "request";
export default {
  name: "Upload",
  data() {
    return {
      songs: null
    };
  },
  methods: {
    uploadFile() {
      console.log("inside log");
      "http://34.89.111.13/v1/songs"
        .then(Response => Response.json())
        .then(json => {
          this.songs = json;
          //this.listConfig = this.songs;
          console.log(this.songs);
        });
    },
    upload() {
      // const req = request.post("http://34.89.111.13/v1/songs/upload", function(err, resp, body) {
      //   if (err) {
      //     console.log("Error!");
      //   } else {
      //     console.log("URL: " + body);
      //   }
      // });
      // const form = req.form();
      // form.append("file", "<FILE_DATA>", {
      //   filename: "myfile.txt",
      //   contentType: "text/plain"
      // });

      const https = require('http');

      const data = "myfile.txt";

      const options = {
          hostname: '34.89.111.13',
          path: '/v1/songs/upload',
          method: 'POST',
          headers: {
              'Content-Type': 'application/octet-stream',
              'Content-Length': data.length
          }
      };

      const req = https.request(options, res => {
          console.log(`statusCode: ${res.statusCode}`);
      });

      req.on('error', error => {
          console.error(error)
      });

      req.write(data);
      console.log("req.write(data)");
      req.end()
    }
  }
};
</script>

<style scoped></style>
