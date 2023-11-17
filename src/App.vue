<template>
  <div class="container">
    <h1>Simon The Game</h1>
    <section>
      <h2 class="loose-message" :class="showLoseMessage ? 'visible' : ''">
        Увы, вы проиграли после {{ score }} раундов
      </h2>
      <div class="game">
        <ul class="game__btn-container">
          <li class="game__btn yellow"></li>
          <li class="game__btn blue"></li>
          <li class="game__btn red"></li>
          <li class="game__btn green"></li>
        </ul>
        <div class="game__options">
          <p>Раунд: {{ score }}</p>
          <button class="start-button" type="button">Начать игру</button>
          <ul>
            Режимы:
            <li>
              <input checked id="easy" type="radio" name="difficulty" />
              <label for="easy">Лёгкий</label>
            </li>
            <li>
              <input id="medium" type="radio" name="difficulty" />
              <label for="medium">Средний</label>
            </li>
            <li>
              <input id="hard" type="radio" name="difficulty" /> <label for="hard">Сложный</label>
            </li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      score: 0,
      showLoseMessage: false,
      delay: 1.5
    };
  }
};
</script>

<style>
.loose-message {
  visibility: hidden;
}

.visible {
  visibility: visible;
}

.yellow {
  background-color: rgb(248, 248, 14);
}

.blue {
  background-color: rgb(14, 14, 245);
}

.red {
  background-color: rgb(249, 24, 24);
}

.green {
  background-color: rgb(10, 125, 10);
}

.container {
  display: flex;
  flex-direction: column;
  padding-top: 100px;
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
  filter: saturate(50%);
}

.game__options {
  text-align: center;
}

p {
  margin-bottom: 25px;
  font-size: larger;
}

.start-button {
  border-radius: 5px;
  padding: 10px 5px;
  margin-bottom: 10px;
  background-color: darkcyan;
  color: white;
  font-size: 18px;
}
</style>
