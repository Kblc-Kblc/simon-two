<template>
  <div class="simon">
    <h1 class="simon__title">Simon game</h1>
    <div class="simon__row">
      <div class="simon__columns circle">
        <div class="circle__row">
          <div class="circle__columns">
            <div
              class="circle__section--one style-circle"
              @click="clicked(1)"
              :class="{ hover: isHover[1] }"
            ></div>
            <div
              class="circle__section--two style-circle"
              @click="clicked(2)"
              :class="{ hover: isHover[2] }"
            ></div>
          </div>
          <div class="circle__columns">
            <div
              class="circle__section--three style-circle"
              @click="clicked(3)"
              :class="{ hover: isHover[3] }"
            ></div>
            <div
              class="circle__section--four style-circle"
              @click="clicked(4)"
              :class="{ hover: isHover[4] }"
            ></div>
          </div>
        </div>
      </div>
      <div class="simon__columns info">
        <div class="info__row">
          <h2 class="info__title">
            {{
              win
                ? "Вы выиграли!!!"
                : gameOver
                ? "Игра окончена"
                : `Раунд ${round}`
            }}
          </h2>
          <a href="#" class="info__btn" @click="startGame">
            <span></span>
            <span></span>
            <span></span>
            <span></span>
            {{ playing ? "Стоп" : "Начать" }}
          </a>
          <h3 class="info__options">Выбери режим :</h3>
          <div class="info__radio">
            <div class="info__radio-wrap">
              <input
                type="radio"
                v-model="selectTime"
                name="1500"
                value="1500"
              />
              <label for="1500"> 1.5c </label>
            </div>
            <div class="info__radio-wrap">
              <input
                type="radio"
                v-model="selectTime"
                name="1000"
                value="1000"
              />
              <label for="1000"> 1c </label>
            </div>
            <div class="info__radio-wrap">
              <input type="radio" v-model="selectTime" name="400" value="400" />
              <label for="400"> 0.4c </label>
            </div>
          </div>
          <button
            type="submit"
            @click="regulationsBtn()"
            class="regulations__btn"
          >
            Правила игры
          </button>
          <regulations v-show="isModalVisible" @close="closeModal" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import regulations from "@/components/Regulations";

export default {
  name: "Simon",
  data() {
    return {
      selectTime: "1500",
      gameOver: false,
      win: false,
      playing: false,
      round: 0,
      showOrder: [],
      clickOrder: [],
      isModalVisible: false,
      isHover: {
        1: false,
        2: false,
        3: false,
        4: false,
      },

      sounds: {
        1: new Audio(require("../audio/1.mp3")),
        2: new Audio(require("../audio/2.mp3")),
        3: new Audio(require("../audio/3.mp3")),
        4: new Audio(require("../audio/4.mp3")),
      },
    };
  },
  methods: {
    regulationsBtn() {
      this.isModalVisible = true;
    },
    startGame() {
      !this.playing
        ? ((this.playing = true),
          this.showed(),
          (this.round = 1),
          clearInterval(this.showOrder))
        : this.stopGame();
    },
    stopGame() {
      this.playing = false;
      this.clickOrder = [];
      this.showOrder = [];
      this.round = 0;
      !this.win && (this.gameOver = true);
      setInterval(() => {
        this.win = false;
        this.gameOver = false;
      }, 1500);
    },
    lightUp(num) {
      this.isHover[num] = true;
      setTimeout(() => {
        this.isHover[num] = false;
      }, 200);
    },
    playSound(num) {
      this.sounds[num].play();
    },
    clicked(num) {
      this.clickOrder.push(num);
      this.lightUp(num);
      this.playSound(num);
      this.clickOrder.length === this.showOrder.length
        ? JSON.stringify(this.clickOrder) === JSON.stringify(this.showOrder)
          ? (this.showed(),
            this.round++,
            this.round === 15 ? ((this.win = true), this.stopGame()) : "")
          : this.stopGame()
        : "";
    },
    showed() {
      let currentIndex = 0;
      this.clickOrder = [];
      this.showOrder.push(Math.floor(Math.random() * 4 + 1));
      this.showInterval = setInterval(() => {
        if (currentIndex >= this.showOrder.length) {
          return clearInterval(this.showInterval);
        }
        this.lightUp(this.showOrder[currentIndex]);
        this.playSound(this.showOrder[currentIndex]);
        currentIndex++;
      }, this.selectTime);
    },
    closeModal() {
      this.isModalVisible = false;
    },
  },
  components: {
    regulations,
  },
};
</script>

<style lang="scss">
.simon {
  padding: 0px 0px 20px 0px;
  &__title {
    text-align: center;
    padding: 5vh 0px 40px 0px;
    font-weight: 700;
    font-size: 30px;
  }

  &__row {
    display: flex;
    flex-wrap: wrap;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  &__columns {
    flex: 1 1 50%;
    padding: 0px 0px 40px 0px;
  }
}
.circle {
  &__row {
    display: flex;
    justify-content: center;
    align-items: center;
    animation: circle 5s linear forwards;
    @keyframes circle {
      0% {
        transform: rotate(0deg);
      }
      100% {
        transform: rotate(1440deg);
      }
    }
  }

  &__columns {
  }

  &__section--one {
    background-color: rgb(255, 0, 0);
    border-radius: 100% 0 0 0;
    border: 2px solid transparent;
    opacity: 0.7;
    &.hover {
      border-color: hsl(0, 100%, 81%);
      filter: drop-shadow(0 0 55px rgb(255, 0, 0));
      opacity: 1;
    }
  }

  &__section--two {
    background: #0075ff;
    border-radius: 0 0 0 100%;
    border: 2px solid transparent;
    opacity: 0.7;

    &.hover {
      border-color: rgb(181, 188, 255);
      filter: drop-shadow(0 0 55px #0075ff);
      opacity: 1;
    }
  }

  &__section--three {
    background-color: rgb(4, 231, 4);
    border-radius: 0 100% 0 0;
    border: 2px solid transparent;
    opacity: 0.7;

    &.hover {
      border-color: rgb(159, 255, 167);
      filter: drop-shadow(0 0 55px rgb(4, 231, 4));
      opacity: 1;
    }
  }

  &__section--four {
    background-color: yellow;
    border-radius: 0 0 100% 0;
    border: 2px solid transparent;
    opacity: 0.7;
    &.hover {
      border-color: #fffed9;
      filter: drop-shadow(0 0 55px yellow);
      opacity: 1;
    }
  }
}
.info {
  padding: 30px 0px 25px 0px;
  border-radius: 8px;
  width: 200px;
  border: 1px solid #fff;

  &__row {
    display: flex;
    flex-direction: column;
    text-align: center;
    flex-wrap: wrap;
  }

  &__title {
    text-align: center;
    text-transform: uppercase;
    font-weight: 600;
    padding: 0px 0px 20px 0px;
    letter-spacing: 3px;
  }

  &__options {
    padding: 20px 0px 10px 0px;
  }

  &__radio {
    padding: 3% 0px 0px 0px;
    margin: 0px auto;
    display: flex;
    flex-direction: column;
    text-align: left;
    flex-wrap: wrap;
  }

  &__radio-wrap {
    padding: 0px 0px 15px 0px;
    input {
      margin: 0px 10px 0px 0px;
    }
  }
}

.style-circle {
  width: 150px;
  height: 150px;
  font-size: 64px;
  text-align: center;
  box-shadow: 5px 10px 10px #323232;
  cursor: pointer;
}

.info__btn {
  position: relative;
  display: inline-block;
  padding: 25px 30px;
  text-decoration: none;
  font-size: 30px;
  transition: 0.5s;
  color: #fff;
  margin: 10px 20px;
  overflow: hidden;
}

.info__btn:hover {
  background-color: #fff;
  color: rgb(0, 0, 31);
  box-shadow: 0px 0px 5px #fff, 0px 0px 25px #fff, 0px 0px 50px #fff,
    0px 0px 100px #fff;
}

/* ANIMATION FOR BUTTON BORDER  */
.info__btn span {
  position: absolute;
  display: block;
}

/* TOP BORDER  */
.info__btn span:nth-child(1) {
  top: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, #fff);
  animation: animate1 1s linear infinite;
}

@keyframes animate1 {
  0% {
    left: -100%;
  }

  50%,
  100% {
    left: 100%;
  }
}

/* RIGHT BORDER */
.info__btn span:nth-child(2) {
  top: 0;
  right: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(180deg, transparent, #fff);
  animation: animate2 1s linear infinite;
  animation-delay: 0.25s;
}

@keyframes animate2 {
  0% {
    top: -100%;
  }

  50%,
  100% {
    top: 100%;
  }
}

/* BOTTOM BORDER  */
.info__btn span:nth-child(3) {
  bottom: 0;
  right: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(360deg, transparent, #fff);
  animation: animate3 1s linear infinite;
  animation-delay: 0.5s;
}

@keyframes animate3 {
  0% {
    right: -100%;
  }

  50%,
  100% {
    right: 100%;
  }
}

/* LEFT BORDER */
.info__btn span:nth-child(4) {
  bottom: 0;
  left: 0;
  width: 2px;
  height: 100%;
  background: linear-gradient(360deg, transparent, #fff);
  animation: animate4 1s linear infinite;
  animation-delay: 0.75s;
}

@keyframes animate4 {
  0% {
    bottom: -100%;
  }

  50%,
  100% {
    bottom: 100%;
  }
}

/* COLOR FOR BUTTON  */
a:nth-child(1) {
  filter: hue-rotate(290deg);
}

a:nth-child(3) {
  filter: hue-rotate(560deg);
}

.regulations__btn {
  display: block;
  padding: 10px 15px;
  max-width: 100px;
  margin: 0px auto;
}
</style>