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
            Spicy jalapeno bacon ipsum dolor amet jerky cupim pork, hamburger chislic rump tri-tip drumstick leberkas spare ribs 
            tongue chicken shank jowl cow. Ham short ribs sausage, meatloaf pork belly venison tail tenderloin chuck. 
            Tri-tip sirloin biltong, ham hock drumstick tenderloin brisket. Short loin capicola doner turducken kevin pig pastrami 
            t-bone andouille pork belly ball tip picanha. Strip steak t-bone cupim tri-tip, shankle meatloaf
          </p>
          <br></br>
          <button @click="open('https://simulatedgreg.gitbooks.io/electron-vue/content/en/')">Read the Docs</button><br><br>
        </div>
        <div class="doc2">
          <div class="title2 alt">External Documentation</div>
          <button class="alt" @click="open('https://electron.atom.io/docs/')">Electron</button>
          <button class="alt" @click="open('https://vuejs.org/v2/guide/')">Vue.js</button>
          <button class="alt" @click="open('https://github.com/SimulatedGREG/electron-vue')">SimulatedGREG</button><br><br>
        </div>
        <div class="hack2">
          <div class="title2 alt">Hack Functions</div>
          <button class="alt2" @click="loadCam">Show Camera</button>
          <button class="alt2" @click="closeCam">Close the *^&^%^ Camera</button>
          <router-link class="alt2" to="/LandingPage" tag="button">Landing Page: Uno</router-link>
        </div>
        <div>
          <video id="video" height="480" width="800" autoplay></video>
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
              maxHeight: 480,
              maxWidth: 800,
              minHeight: 480,
              minWidth: 800
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
    height: 100vh;
    padding: 60px 80px;
    width: 100vw;
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
    padding: 0.75em 2em;
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
</style>
