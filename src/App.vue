<template>
  <div>
    <ScoreBoard :me="me" :pc="pc" />
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>
      <template v-for="(item, idx) in this.answers" :key="idx">
        <input :disabled="submited" type="radio" name="options" :value="item" v-model="choosen" />
        <label v-html="item"></label>
        <br />
      </template>
      <button class="send" v-if="!submited" @click="submitChoosen()" type="button">
        Send
      </button>

      <section class="result" v-if="submited">
        <h4 v-if="isCorrect">Resposta Correta</h4>
        <h4 v-else>
          Resposta Errada. A resposta correta é
          <label v-html="this.correctAnswers"></label>
        </h4>
        <button class="send" @click="getNew()" type="button">
          Proxima pergunta
        </button>
      </section>
    </template>
  </div>
</template>

<script>
import ScoreBoard from '@/components/ScoreBoard.vue';
export default {
  name: 'App',
  components: {
    ScoreBoard,
  },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswers: undefined,
      choosen: undefined,
      isCorrect: false,
      submited: false,
      me: 0,
      pc: 0,
    };
  },
  methods: {
    init: function () {
      this.submited = false;
      this.choosen = undefined;
      this.question = undefined;
      this.isCorrect = false;
    },
    getNew: function () {
      this.init();
      this.axios
        .get('https://opentdb.com/api.php?amount=1')
        .then((response) => {
          this.question = response.data.results[0].question;
          this.incorrectAnswers = response.data.results[0].incorrect_answers;
          this.correctAnswers = response.data.results[0].correct_answer;
        });
    },
    submitChoosen: function () {
      if (this.choosen) {
        this.submited = true;
        this.isCorrect = this.choosen == this.correctAnswers;
        this.isCorrect ? this.me++ : this.pc++;
      } else {
        alert('informe uma resposta');
      }
    },
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(
        Math.round(Math.random() * answers.length),
        0,
        this.correctAnswers
      );
      return answers;
    },
  },
  created() {
    this.getNew();
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type='radio'] {
    margin: 12px 5px;
  }

  button.send {
    background-color: #1867c0;
    border: 1px solid #1867c0;
    color: #fff;
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    cursor: pointer;
    padding: 0 16px;
  }
}
</style>
