<template>
  <div id="home">
    <Particles
      id="tsparticles"
      :particlesInit="particlesInit"
      :particlesLoaded="particlesLoaded"
      :options="{
        background: {
          color: {
            value: '#000',
          },
        },
        fpsLimit: 60,
        interactivity: {
          detectsOn: 'canvas',
          events: {
            onHover: {
              enable: true,
              mode: 'bubble',
            },
            resize: true,
          },
          modes: {
            bubble: {
              distance: 200,
              duration: 2,
              opacity: 0.8,
              size: 10,
            },
          },
        },
        particles: {
          color: {
            value: '#ffffff',
          },
          links: {
            color: '#ffffff',
            distance: 100,
            enable: true,
            opacity: 0.5,
            width: 0.5,
          },
          collisions: {
            enable: true,
          },
          move: {
            direction: 'none',
            enable: true,
            outMode: 'bounce',
            random: true,
            speed: 1,
            straight: false,
          },
          number: {
            value: 180,
          },
          opacity: {
            value: 0.3,
          },
          shape: {
            type: 'star',
            size: 1,
          },
          size: {
            random: true,
            value: 5,
          },
        },
        detectRetina: true,
      }"
    >
    </Particles>
    <div class="main-section">
      <section class="flags">
        <img src="@/assets/eua.png" alt="" />
        <label class="switch">
          <input
            @click="changeLanguage"
            type="checkbox"
            v-model="switchButton"
          />
          <span class="slider round"></span>
        </label>
        <img src="@/assets/brasil.png" alt="" />
      </section>
      <section class="content">
        <!-- pt-br -->
        <span  v-if="switchButton == true" class="instructions"
          >Clique no botão abaixo para gerar sua mensagem do dia!</span
        >
        <div v-if="clicked && switchButton == true">

        <span class="quote-container">{{ frases }}</span>
        </div>
        <button
          @click="fetchDataBR"
          v-if="switchButton == true"
          class="button-style magic"
        >
          Gerar frase
        </button>

        <!-- en-us -->
        <span v-if=" switchButton == false" class="instructions"
          >Click on the button below to generate your message of the day!</span
        >
        <div class="instructions" v-if="clicked && switchButton == false">
        <span class="quote-container">{{ quotes }}</span>
        <small class="small-container">{{ author }}</small>

        </div>
        <button
          @click="fetchDataUS"
          v-if="switchButton == false"
          class="button-style magic"
        >
          Generate quote
        </button>
      </section>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      clicked: false,
      quotes: null,
      frases: null,
      switchButton: false,
      author: null,
      autor: null,
    };
  },
  created() {},
  computed: {
    classes() {
      return this.isloading ? "is-blue" : "is-red";
    },
  },
  methods: {
    changeLanguage() {
      this.switchButton = !this.switchButton;
    },
    fetchDataBR() {
      axios
        .get(`localhost:8081/api/quotes`)
        .then((response) => {
          let dados = response;
          console.log(dados)
          // this.autor = response.author;
          // this.frases = response.quote;
        });
      this.clicked = true;
    },
    fetchDataUS() {
      axios.get(`https://api.quotable.io/random`).then((response) => {
        let data = response.data;
        this.author = data.author;
        this.quotes = data.content;
      });
      this.clicked = true;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Raleway&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  font-family: "Raleway", sans-serif;
}
.instructions {
  color: #fff;
  display:flex;
  flex-direction:column
}
.flags {
  margin-top: 20px;
  margin-left: 10px;
  height: 50px;
  display: flex;
}
.flags img {
  margin: 0px 10px 0px 10px;
  height: 30px;
}
#home {
  height: 100vh;
  width: 100vw;
  background: #000;
}
#tsparticles {
  height: 100vh;
  width: 100%;
  position: absolute;
  background-color: #000000;
  background-size: cover;
  background-position: 100% 100%;
}
.main-section {
  flex: 1;
  height: 600px;
  width: 900px;
  background: #ffffff1a;
  position: absolute;
  border-radius: 40px;
  z-index: 2;

  backdrop-filter: blur(7px);
  margin-left: auto;
  margin-right: auto;
  top: 10%;
  left: 0;
  right: 0;
  text-align: center;
  box-shadow: 20px 20px 50px #0a0a0a40;
  border-left: 1px solid #bfbfff40;
}
.content {
  display: flex;
  justify-content: space-evenly;
  flex-direction: column;
  align-items: center;
  height: 100%;
}
.quote-container {
  height: 100px;
  width: 100%;
  color: #fff;
  font-size: 1.5em;
  padding: 25px;
}
.small-container {
  color: rgb(136, 136, 136);
  font-size: 1em;
  font-weight: 400;
}
/* buttons */
.button-style {
  outline: none;
  text-decoration: none;
  height: 50px;
  width: 200px;
  border-radius: 6px;
  background: grey;
  border: none;
  padding: 0;
  cursor: pointer;
  color: #fff;
  text-transform: uppercase;
  font-weight: bold;
}
.button-style:hover {
  background: rgb(88, 88, 88);
}
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgb(253, 132, 132);
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}
input:checked + .slider {
  background-color: #ddf768;
}

input:focus + .slider {
  box-shadow: 0 0 1px #818a7b;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
/* animations */
.magic {
  display: inline-block;
  margin: 50px;
  position: relative;
}

.magic i {
  color: orange;
  filter: grayscale(100%);
}
.magic:hover i {
  animation: change 1s forwards;
}

@keyframes change {
  50% {
    transform: scale(0);
    filter: grayscale(100%);
  }
  51% {
    filter: grayscale(0%);
  }
  100% {
    transform: scale(1);
    filter: grayscale(0%);
  }
}

.magic:before {
  content: "";
  position: absolute;
  top: calc(50% - 45px);
  left: calc(50% - 45px);
  width: 90px;
  height: 90px;
  border-radius: 50%;
  border-color: rgb(255, 238, 208);
  border-style: solid;
  border-width: 45px;
  transform: scale(0);
  box-sizing: border-box;
}

.magic::after,
.magic i::after {
  content: "";
  position: absolute;
  width: 160px;
  height: 160px;
  left: calc(50% - 80px);
  top: calc(50% - 80px);
  background:
    /*4 reds*/ radial-gradient(
      circle,
      rgb(255, 255, 255) 50%,
      transparent 60%
    ),
    radial-gradient(circle, rgb(255, 255, 255) 50%, transparent 60%),
    radial-gradient(circle, rgb(180, 180, 180) 50%, transparent 60%),
    radial-gradient(circle, rgb(177, 175, 175) 50%, transparent 60%),
    /*4 oranges*/ radial-gradient(circle, rgb(71, 71, 71) 50%, transparent 60%),
    radial-gradient(circle, rgb(73, 73, 73) 50%, transparent 60%),
    radial-gradient(circle, rgb(58, 58, 58) 50%, transparent 60%),
    radial-gradient(circle, rgb(29, 29, 29) 50%, transparent 60%);

  background-size: 16px 16px;
  background-position: calc(50% - 50px) calc(50% - 50px),
    calc(50% + 50px) calc(50% - 50px), calc(50% - 50px) calc(50% + 50px),
    calc(50% + 50px) calc(50% + 50px), calc(50% + 0px) calc(50% + 70px),
    calc(50% + 70px) calc(50% + 0px), calc(50% - 70px) calc(50% + 0px),
    calc(50% + 0px) calc(50% - 70px);
  background-repeat: no-repeat;
  border-radius: 50%;
  transform: scale(0);
}
.magic i::after {
  background-size: 10px 10px;
  transform: rotate(10deg) scale(0);
}

.magic:hover:after {
  transform: scale(1);
  opacity: 0;
  background-size: 0 0;
  transition: transform 0.5s 0.5s, opacity 0.4s 0.9s, background-size 0.5s 0.9s;
}
.magic:hover i:after {
  transform: rotate(10deg) scale(1);
  opacity: 0;
  background-size: 0 0;
  transition: transform 0.5s 0.5s, opacity 0.4s 0.9s, background-size 0.5s 0.9s;
}
</style>
