<template>
  <div class="container">
    <h1>Simon The Game</h1>
    <section>
      <h2 class="loose-message" :class="showLoseMessage ? 'visible' : ''">
        Увы, вы проиграли после {{ score }} раунд(-ов/-а)
      </h2>
      <div class="game">
        <ul class="game__btn-container">
          <li ref="1" class="game__btn yellow" @click="gameActive ? activateBtn(1) : ''"></li>
          <li ref="2" class="game__btn blue" @click="gameActive ? activateBtn(2) : ''"></li>
          <li ref="3" class="game__btn red" @click="gameActive ? activateBtn(3) : ''"></li>
          <li ref="4" class="game__btn green" @click="gameActive ? activateBtn(4) : ''"></li>
        </ul>
        <div class="game__options">
          <p>Раунд: {{ score }}</p>
          <button
            :disabled="gameActive"
            class="button start-button"
            type="button"
            @click="startGame"
          >
            Начать игру
          </button>
          <ul>
            <li>
              <label for="easy">
                <input
                  id="easy"
                  type="radio"
                  :disabled="gameActive"
                  name="difficulty"
                  v-model="delay"
                  value="1500"
                />
                Лёгкий
              </label>
            </li>
            <li>
              <label for="medium">
                <input
                  id="medium"
                  type="radio"
                  :disabled="gameActive"
                  name="difficulty"
                  v-model="delay"
                  value="1000"
                />
                Средний
              </label>
            </li>
            <li>
              <label for="hard">
                <input
                  id="hard"
                  type="radio"
                  :disabled="gameActive"
                  name="difficulty"
                  v-model="delay"
                  value="400"
                />
                Сложный
              </label>
            </li>
          </ul>
          <button
            :disabled="!gameActive"
            class="button restart-button"
            @click="startGame"
            type="button"
          >
            Заново
          </button>
          <button
            v-if="gameActive"
            class="button change-difficulty-button"
            @click="resetTheGame"
            type="button"
          >
            Сменить сложность
          </button>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
import audioYellow from './sounds/1.mp3';
import audioBlue from './sounds/2.mp3';
import audioRed from './sounds/3.mp3';
import audioGreen from './sounds/4.mp3';
export default {
  name: 'app',
  data() {
    return {
      gameActive: false,
      score: 0,
      showLoseMessage: false,
      delay: 1500,
      gameSequence: [],
      playerSequence: [],
      pushes: 0
    };
  },
  methods: {
    activateBtn(number, isGamePlaying) {
      if (!isGamePlaying) {
        this.playerSequence.push(number);
        this.playSound(number);
        const areSequencesDifferent = this.compareSequences();
        this.pushes++;

        if (areSequencesDifferent) {
          this.resetTheGame();
          this.showLoseMessage = true;
        } else if (this.playerSequence.length === this.gameSequence.length) {
          this.score += 1;
          this.pushes = 0;
          this.playerSequence = [];
          this.createGameSequence();
          setTimeout(() => {
            this.fireButtons();
          }, this.delay);
        } else {
          return;
        }
      } else {
        this.playSound(number);
        setTimeout(() => {
          this.$refs[number].classList.remove('active');
        }, 200);
        this.$refs[number].classList.add('active');
      }
    },
    playSound(number) {
      const audio = {
        1: () => new Audio(audioYellow),
        2: () => new Audio(audioBlue),
        3: () => new Audio(audioRed),
        4: () => new Audio(audioGreen)
      };
      audio[number]().play();
    },
    fireButtons() {
      this.gameSequence.forEach((num, i) => {
        setTimeout(() => {
          this.activateBtn(num, true);
        }, i * this.delay);
      });
    },
    createGameSequence() {
      this.gameSequence.push(Math.ceil(Math.random() * 4));
    },
    compareSequences() {
      if (this.playerSequence[this.pushes] === this.gameSequence[this.pushes]) {
        return false;
      } else {
        return true;
      }
    },
    resetAllData() {
      this.showLoseMessage = false;
      this.gameSequence = [];
      this.playerSequence = [];
      this.score = 1;
      this.pushes = 0;
    },
    resetTheGame() {
      this.gameActive = false;
      this.score = 0;
      this.psuhes = 0;
    },
    startGame() {
      this.gameActive = true;
      this.resetAllData();
      this.createGameSequence();
      this.fireButtons();
    }
  }
};
</script>

<style>
@keyframes bounce {
  0%,
  20%,
  50%,
  80%,
  100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-20px);
  }
  60% {
    transform: translateY(-10px);
  }
}
.loose-message {
  visibility: hidden;
  text-align: center;
  animation: bounce 1s 0.5s infinite;
}

h1 {
  margin-bottom: 30px;
}

.visible {
  visibility: visible;
}

.yellow {
  background-color: rgb(250, 250, 95);
}

.blue {
  background-color: rgb(94, 94, 248);
}

.red {
  background-color: rgb(253, 90, 90);
}

.green {
  background-color: rgb(61, 168, 61);
}

.container {
  display: flex;
  flex-direction: column;
  padding-top: 50px;
  align-items: center;
}

.game {
  display: flex;
  max-width: 800px;
  margin-inline: auto;
  margin-top: 50px;
  gap: 50px;
}

.game__btn-container {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
  border-radius: 100%;
  border: 3px rgb(57, 57, 57) solid;
}

.game__btn {
  border-radius: 100%;
  border: 2px black solid;
  width: 10vw;
  height: 10vw;
  cursor: pointer;
}

.game__btn:hover {
  filter: saturate(300%);
}

.active {
  filter: saturate(300%);
}

.game__options {
  text-align: center;
  display: flex;
  flex-direction: column;
}

p {
  margin-bottom: 25px;
  font-size: larger;
}

.button {
  border-radius: 5px;
  padding: 10px 10px;
  margin-bottom: 10px;
  margin-top: 10px;
  border: 1px grey solid;
  font-size: 16px;
  max-width: 127px;
}

.button:disabled {
  cursor: auto;
}

.start-button {
  background-color: darkcyan;
  color: white;
  font-size: 18px;
  margin-top: 0;
  margin-bottom: 10px;
  border: none;
}
</style>
