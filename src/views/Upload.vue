<template>

  <div class="container">
    <div v-on:click="upload()">dfgdfgdfg</div>
    <div class="large-12 medium-12 small-12 cell">
      <label>File
        <input type="file" id="file" ref="file" v-on:change="handleFileUpload()"/>
      </label>
      <button v-on:click="submitFile()">Submit</button>
    </div>
  </div>
</template>

<script>
  // const request = require('request');
  import request from "request";


  export default {
    name: "Upload",
    data() {
      return {
        songs: null,
        file: ''
      };
    },
    loaded() {
      this.upload();
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

        const https = require("http");
        const fs = require('fs');

        const data = "Synymata.mp3";

        // const fs = require("fs");
        // const binary = fs.readFileSync(data, 'binary');

        //const fs = require("fs");
        console.log("\n *STARTING* \n");
        const contents = fs.readFileSync('./Synymata.mp3', 'binary');

        console.log(contents.length);

        const options = {
          hostname: "34.89.111.13",
          path: "/v1/songs/upload",
          method: "POST",
          headers: {
            "Content-Type": "application/octet-stream",
            "Content-Length": contents.length
          }
        };

        const req = https.request(options, res => {
          console.log(`statusCode: ${res.statusCode}`);
        });

        req.on("error", error => {
          console.error(error);
        });

        req.write(contents);
        console.log("req.write(contents)");
        req.end();
      },
      handleFileUpload(){
        this.file = this.$refs.file.files[0];
      },
      submitFile(){
        let formData = new FormData();
        formData.append('file', this.file);
        axios.post( '/single-file',
          formData,
          {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          }
        ).then(function(){
          console.log('SUCCESS!!');
        })
          .catch(function(){
            console.log('FAILURE!!');
          });
      }
    }
  };
</script>

<style scoped></style>
