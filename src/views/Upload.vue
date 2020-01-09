<template>

  <div class="container">
    <div v-on:click="upload()">dfgdfgdfg</div>
    <div class="large-12 medium-12 small-12 cell">
      <label>File
        <input type="file" id="file" ref="file" v-on:change="handleFileUpload()"/>
<!--        <input type="file" @change="uploadImage($event)">-->
      </label>
      <button v-on:click="submitFile()">Submit</button>
    </div>
    <div class="form-group">
      <label for="songName">Song name</label>
      <input type="text"
             id="songName"
             class="form-control"
             v-model="song.songName">
      <label for="authorName">Author name</label>
      <input type="text"
             id="authorName"
             class="form-control"
             v-model="song.authorId">
      <label for="albumName">Album name</label>
      <input type="text"
             id="albumName"
             class="form-control"
             v-model="song.albumId">
      <button class="btn btn-primary"
              @click.prevent="submitted">Subm it!
      </button>
    </div>

  </div>
</template>

<script>
  import axios from 'axios';
  // const request = require('request');
  import request from "request";


  export default {
    name: "Upload",
    data() {
      return {
        songs: null,
        file: '',
        song: {
          songName: '',
          authorId: '',
          albumId: ''
        }
      };
    },
    loaded() {
      this.upload();
    },
    methods: {
      uploadUserImage: function (formData) {
        axios.post('http://34.89.111.13/v1/songs/upload', formData,
          {
            headers: {
              'Content-Type': 'application/octet-stream'
            }
          })
          .then(function (response) {
            console.log(response)
          })
      },
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
      uploadRequest(){
        const options = {
          method: "POST",
          url: "http://34.89.111.13/v1/songs/upload",
          headers: {
            "Content-Type": "application/octet-stream"
          },
          formData : {
            "image" : fs.createReadStream("./images/scr1.png")
          }
        };

        request(options, function (err, res, body) {
          if(err) console.log(err);
          console.log(body);
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
        //const axios = require('axios');

        let formData = new FormData();
        formData.append('file', this.file);
        axios.post( 'http://34.89.111.13/v1/songs/upload',
          formData,
          {
            headers: {
              'Content-Type': 'application/octet-stream'
            }
          }
        ).then(function(){
          console.log('SUCCESS!!');
        })
          .catch(function(){
            console.log('FAILURE!!');
          });
      },
      uploadImage (e) {
        this.file = e.currentTarget.files[0]
        let formData = new FormData()
        formData.append('img', this.file)
        this.uploadUserImage(formData)
      }
    }
  };
</script>

<style scoped></style>
