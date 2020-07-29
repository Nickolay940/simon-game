<template>
  <div class="game-field">
    <div class="circle-container">
      <div class="left-col">
        <div class="circle left-circle-top" id="yellow" @click="handleColorButton"></div>
        <div class="circle left-circle-bottom" id="red" @click="handleColorButton"></div>
      </div>
      <div class="rigth-col">
        <div class="circle rigth-circle-top" id="blue" @click="handleColorButton"></div>
        <div class="circle rigth-circle-bottom" id="green" @click="handleColorButton"></div>
      </div>
    </div>
    <div class="panel">
      <h2>Round: {{count}}</h2>
      <button class="start-button" @click="start">Start</button>
      <h2>Game options:</h2>
      <div class="inputs-container">
        <div class="input-field">
          <input type="radio" value="1500" v-model="complexity" />
          <label>Легкий</label>
        </div>

        <div class="input-field">
          <input type="radio" value="1000" v-model="complexity" />
          <label>Средний</label>
        </div>

        <div class="input-field">
          <input type="radio" value="400" v-model="complexity" />
          <label>Сложный</label>
        </div>
      </div>
      <span class="faild-msg">К сожалению, вы ошиблись. Игра окончена.</span>
    </div>
  </div>
</template>

<script>
import "../stylus/style.styl";

const simonSound1 = require("@/assets/sounds/simonSound1.mp3");
const simonSound2 = require("@/assets/sounds/simonSound2.mp3");
const simonSound3 = require("@/assets/sounds/simonSound3.mp3");
const simonSound4 = require("@/assets/sounds/simonSound4.mp3");

export default {
  name: "GameField",
  data: () => ({
    complexity: "1500",
    count: "0",
    step: 0,
    playerTurn: false,
    maxLevel: 10,
    steps: [],
    playerSteps: [],
    faildMsg: false,
  }),
  methods: {
    // on color button click
    handleColorButton(event) {
      if (this.playerTurn) {
        this.audioSignal(event.target.id);
        this.lightColorButton(event.target.id);
        this.playerSteps.push(event.target.id);
        if (this.compareSequence(this.step)) {
          this.step++;
          if (this.step >= this.steps.length && this.steps.length < 20) {
            this.playerTurn = false;
            setTimeout(() => {
              this.addStep();
              this.playSequence(this.steps, this.complexity);
              this.playerSteps = [];
              this.step = 0;
            }, 1250);
          }
        } else {
          this.playerTurn = false;
          this.count = 0;
          this.showFailedMsg();
          return;
        }
      }
    },

    // on start button
    start() {
      if (this.faildMsg) {
        this.clearFalidMsg();
      }
      this.count = 0;
      this.step = 0;
      this.steps = [];
      this.playerSteps = [];
      this.addStep();
      this.playSequence(this.steps, this.complexity);
    },

    // compares game and player steps
    compareSequence(step) {
      let game = this.steps.slice();
      let player = this.playerSteps.slice();
      return game[step] === player[step];
    },

    //add step
    addStep() {
      switch (Math.floor(Math.random() * 4)) {
        case 0:
          this.steps.push("red");
          break;
        case 1:
          this.steps.push("green");
          break;
        case 2:
          this.steps.push("blue");
          break;
        case 3:
          this.steps.push("yellow");
          break;
      }
      this.count++;
    },

    playSequence(sequence, delay) {
      if (this.steps.length === this.maxLevel) {
        this.playerTurn = false;
        this.count = "Победа";
      } else {
        for (let i = 0; i < sequence.length; i++) {
          setTimeout(() => {
            this.audioSignal(sequence[i]);
            this.lightColorButton(sequence[i]);
          }, i * delay);
        }
        this.playerTurn = true;
      }
    },

    audioSignal(color) {
      const redAudio = new Audio(simonSound1);
      const greenAudio = new Audio(simonSound2);
      const blueAudio = new Audio(simonSound3);
      const yellowAudio = new Audio(simonSound4);

      switch (color) {
        case "red":
          redAudio.play();
          break;
        case "green":
          greenAudio.play();
          break;
        case "blue":
          blueAudio.play();
          break;
        case "yellow":
          yellowAudio.play();
          break;
      }
    },

    lightColorButton(color) {
      document.getElementById(color).classList.toggle("active");
      setTimeout(() => {
        document.getElementById(color).classList.toggle("active");
      }, 500);
    },

    //show faild msg
    showFailedMsg() {
      document.querySelector(".faild-msg").classList.add("show-msg");
      this.faildMsg = true;
    },

    clearFalidMsg() {
      document.querySelector(".faild-msg").classList.remove("show-msg");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="stylus"></style>
