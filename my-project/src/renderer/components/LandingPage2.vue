<template>
  <div id="wrapper2">
    <img id="logo" src="~@/assets/logo2.png" alt="RecTrac">
    <main>
      <div class="left-side">
        <span class="title3">
        &nbsp &nbsp MAKE IT BETTER.<BR></BR>
        &nbsp &nbsp MAKE IT SIMPLER.<BR></BR>
        &nbsp &nbsp MAKE A DIFFERENCE FOR OUR CUSTOMERS.
        </span>
        <system-information2></system-information2>
      </div>

      <div class="right-side">
        <div class="doc2">
          <div class="title2">Fun with Cameras!</div>
          <p>
            Project Overview
            <br></br>
            This is where we tell them what we will tell them 
          </p>
        </div>
        <br></br>
        <div class="doc2">
          <div class="title2 alt">External Documentation</div>
          <button class="alt" @click="open('https://electron.atom.io/docs/')">Electron</button>
          <button class="alt" @click="open('https://vuejs.org/v2/guide/')">Vue.js</button>
          <button class="alt" @click="open('https://github.com/SimulatedGREG/electron-vue')">SimulatedGREG</button><br><br>
        </div>
        <div class="hack2">
          <div class="title2 alt">Hack Functions</div>
          <button id="loadCam" class="alt2" @click="loadCam">Show Camera</button>
          <button id="closeCam" class="alt2" @click="closeCam">Close the *^&^%^ Camera</button>
          <button id="snapPic" class="alt2" @click="snapPic">Take a Pic</button>
          <button id="savePic" class="alt2" @click="savePic">Save Pic</button>
          <router-link class="alt2" to="/LandingPage" tag="button">Landing Page: Uno</router-link>
        </div>
        <div>
          <video id="video" height="480" width="320" autoplay></video>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
  import SystemInformation2 from './LandingPage/SystemInformation2'

  export default {
    name: 'landing-page2',
    components: { SystemInformation2 },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      loadCam () {
        const constraints = {
          audio: false,
          video: {
            mandatory: {
              maxHeight: document.querySelector('video').getAttribute('height'),
              maxWidth: document.querySelector('video').getAttribute('width'),
              minHeight: document.querySelector('video').getAttribute('height'),
              minWidth: document.querySelector('video').getAttribute('width')
            }
          }
        }

        navigator.getUserMedia(constraints,
          function (stream) {
            var video = document.querySelector('video')
            video.srcObject = stream
            video.onloadedmetadata = function (e) {
              video.play()
            }
          },
          function (err) {
            console.log('The following error occurred: ' + err.name)
          }
        )
      },
      closeCam () {
        const videoElement = document.getElementById('video')

        let stream = videoElement.srcObject
        let tracks = stream.getTracks()

        tracks.forEach(function (track) {
          track.stop()
        })

        videoElement.srcObject = null
      },
      snapPic () {
        var video = document.querySelector('video')
        var canvas = document.querySelector('canvas')
        var context
        var width = video.getAttribute('width')
        var height = video.getAttribute('height')

        canvas = canvas || document.createElement('canvas')
        canvas.width = width
        canvas.height = height

        context = canvas.getContext('2d')
        context.drawImage(video, 0, 0, 320, 480)
      },
      savePic () {
        // Get the DataUrl from the Canvas
        var canvas = document.querySelector('canvas')
        var fs = require('fs')
        const url = canvas.toDataURL('image/jpg', 0.8)
        var filePath = 'myImage.jpg'
        // remove Base64 stuff from the Image
        const base64Data = url.replace(/^data:image\/png;base64,/, '')
        fs.writeFile(filePath, base64Data, 'base64', function (err) {
          console.log(err)
          var ctx = canvas.getContext('2d')
          ctx.fillStyle = 'rgba(0, 255, 0, 0.5)'
          ctx.fillRect(0, 0, 320, 480)
          ctx.font = '40px serif'
          ctx.fillStyle = 'white'
          ctx.fillText('Photo Saved', 30, 100)
        })
      }
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }

  #wrapper2 {
    background:
      radial-gradient(
        ellipse at top left,
        rgb(1,92, 142) 40%,
        rgba(255, 255, 255, .9) 100%
      );
    height: 100%;
    padding: 60px 80px;
    width: 100%;
  }

  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  main {
    display: flex;
    justify-content: space-between;
  }

  main > div { flex-basis: 50%; }

  .left-side {
    display: flex;
    flex-direction: column;
  }

  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title2 {
    color: #ffffff;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
  }
  .title3 {
    color: #ffffff;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
    transform: skewX(-20deg);
    border-left: solid white 2px; 
    text-indent: 10px hanging ;
  }

  .title2.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .doc p {
    color: black;
    margin-bottom: 10px;
  }

  .doc2 button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #215fab;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #215fab;
  }

  .doc2 button.alt {
    color: #ffffff;
    border:1px solid rgba(255, 255, 255, .9);
    background-color: transparent;
  }

  .hack p {
    color: black;
    margin-bottom: 10px;
  }

  .hack2 button {
    cursor: pointer;
    outline: #2c3e50;
    padding: .75em 2em;
    margin: 5px;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #215fab;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 2px solid #215fab;
  }

  .hack button.alt2 {
    color: #ffffff;
    background-color: transparent;
  }

  #video {
    padding-top:5px;
  }
  ul {
  display: block;
  list-style-type: square;
  margin-top: 1em;
  margin-bottom: 1 em;
  margin-left: 0;
  margin-right: 0;
  padding-left: 40px;
}

</style>
