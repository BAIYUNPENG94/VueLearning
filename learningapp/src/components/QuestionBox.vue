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
          :class="[selectedindex === index ? 'selected' : '']"
        >{{ answer }}</b-list-group-item>
      </b-list-group>
      <b-button variant="primary" href="#">SUBMIT</b-button>
      <b-button @click="next" variant="success" href="#">Next One</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
export default {
  props: {
    currentquestion: Object,
    next: Function
  },
  data() {
    return {
      selectedindex: null
    };
  },
  methods: {
    selectanswer: function(index) {
      this.selectedindex = index;
      console.log(index);
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
  background-color: blue;
}

.correctanswer {
  background-color: green;
}

.wrong answer {
  background-color: red;
}
</style>