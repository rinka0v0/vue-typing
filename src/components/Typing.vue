<template>
  <div class="container">
    <div class="title">
      <h1>Typing Game</h1>
      <div class="marker"></div>
    </div>
    <button
      class="startButton mb-20"
      @click="gameStart"
      v-if="startFlag !== true"
    >
      START
    </button>
    <div v-if="startFlag">
      <div class="question mb-20">{{ current_question }}</div>
      <div class="clear" v-if="current_question_counts === question_counts">
        Clear!
      </div>
      <div class="typeFormWrapper mb-20">
        <input id="typeForm" type="text" class="typeForm" v-model="typeBox" />
      </div>
      <div class="gaugeWrapper mb-20">
        <div class="gauge" v-bind:style="styleObject"></div>
      </div>
      <div>{{ current_question_counts }}/{{ question_counts }}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      startFlag: false,
      current_question: "",
      questions: ["apple", "banana", "chocolate", "donut", "espresso"],
      typeBox: "",
      current_question_counts: 0,
      question_counts: 0,
    };
  },
  computed: {
    styleObject: function() {
      const width = 20 * this.current_question_counts + "%";
      let color = "";
      if (this.current_question_counts === this.question_counts) {
        color = "#03a9f4";
      } else {
        color = "orange";
      }
      return { width: width, "background-color": color };
    },
  },
  methods: {
    gameStart() {
      this.startFlag = true;
      this.$nextTick(function() {
        document.getElementById("typeForm").focus();
      });
    },
  },
  mounted: function() {
    this.current_question = this.questions[0];
    this.question_counts = this.questions.length;
  },
  watch: {
    typeBox: function(e) {
      if (e == this.current_question) {
        this.questions.splice(0, 1);
        this.current_question = this.questions[0];
        this.typeBox = "";
        this.current_question_counts = this.current_question_counts + 1;
      }
    },
  },
};
</script>

<style>
* {
  font-size: inherit;
  line-height: inherit;
}
body {
  font-size: 32px;
}

.mb-20 {
  margin-bottom: 20px;
}

.container {
  width: 400px;
  margin: 0 auto;
  text-align: center;
}

.title {
  position: relative;
  font-size: 48px;
}

.marker {
  width: 100%;
  height: 35%;
  background-color: #a2a2a270;
  position: absolute;
  bottom: 5%;
  z-index: -1;
}

.startButton {
  background-color: #333;
  color: #fff;
  padding: 4px 60px;
  border: none;
  outline: none;
  border-radius: 8px;
  cursor: pointer;
}

.startButton:hover {
  opacity: 0.7;
}

.question {
  color: gray;
}

.clear {
  color: #03a9f4;
}

.typeForm {
  text-align: center;
  outline: none;
  border: none;
}

.typeFormWrapper {
  border-bottom: 1px solid gray;
}

.gauge {
  height: 12px;
  transition: all 0.3s ease;
}

.gaugeWrapper {
  border: 1px solid;
  height: 12px;
}
</style>
