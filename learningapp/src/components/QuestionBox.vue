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
          :class="answerClass(index)"
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
    },
    answerClass: function(index) {
      let answerClass = "";
      if (this.selectedindex === index && !this.answered) {
        answerClass = "selected";
      } else if (this.answered && this.correct_index === index) {
        answerClass = "correctanswer";
      } else if (
        this.answered &&
        this.selectedindex === index &&
        this.correct_index !== this.selectedindex
      ) {
        answerClass = "wronganswer";
      }

      return answerClass;
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