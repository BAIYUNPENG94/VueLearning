<template>
  <div class>
    <b-jumbotron>
      <template v-slot:lead>{{ currentquestion.question }}</template>
      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in answers"
          :key="index"
          @click.prevent="selectanswer(index)"
          :class="[!answered && selectedindex === index ? 'selected':
          answered && correct_index === index ? 'correctanswer':
          answered && selectedindex === index && correct_index !== selectedindex ? 'wronganswer':'']"
        >{{ answer }}</b-list-group-item>
      </b-list-group>
      <b-button
        @click="submitAnswer"
        variant="primary"
        :disabled="selectedindex == null || answered"
      >SUBMIT</b-button>
      <b-button @click="next" variant="success" href="#">Next One</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentquestion: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      correct_index: null,
      selectedindex: null,
      shuffledanswers: [],
      answered: false
    };
  },
  methods: {
    selectanswer: function(index) {
      this.selectedindex = index;
      console.log(index);
    },
    submitAnswer: function() {
      let iscorrect = false;
      if (this.selectedindex === this.correct_index) {
        iscorrect = true;
      }
      this.answered = true;
      this.increment(iscorrect);
    },
    shuffleanswers: function() {
      let answers = [
        ...this.currentquestion.incorrect_answers,
        this.currentquestion.correct_answer
      ];
      this.shuffledanswers = _.shuffle(answers);
      this.correct_index = this.shuffledanswers.indexOf(
        this.currentquestion.correct_answer
      );
    }
  },
  watch: {
    currentquestion: {
      immediate: true,
      handler() {
        this.selectedindex = null;
        this.shuffleanswers();
        this.answered = false;
      }
    }
  },
  computed: {
    answers: function() {
      let answers = [...this.currentquestion.incorrect_answers];
      answers.push(this.currentquestion.correct_answer);
      return answers;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}

.list-group-item:hover {
  background-color: gray;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: lightblue;
}

.correctanswer {
  background-color: lightgreen;
}

.wronganswer {
  background-color: lightsalmon;
}
</style>