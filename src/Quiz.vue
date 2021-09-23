<template>
  <div>
    <div class="imageBox"></div>
    <h1 class="title">General Questionnaire</h1>

    <!--------------------------------------------------------------------->
    <!-- Welcome Page ----------------------------------------------------->
    <div class="welcomePage" v-if="quizStart">
      <!-- Status bar -->
      <div class="statusBar">
        <p class="statusBarText">
          <b>Intro ></b> Question&emsp14;1 > Question&emsp14;2 >
          Question&emsp14;3 > Question&emsp14;4 > Question&emsp14;5 > Summary
        </p>
      </div>
      <!-- Welcome Page content -->
      <h3 class="header">Welcome to the General Questionnaire!</h3>
      <p class="centerText">
        This is a fun questionnaire to get to know you and test your knowledge!
        There will be 5 questions, and you cannot revisit any questions once you
        have already answered them. If you are ready, please begin using the
        button below!
      </p>
      <button class="button" @click="startQuiz">Get Started</button>
    </div>

    <!--------------------------------------------------------------------->
    <!-- General Questions Page ------------------------------------------->
    <div v-else-if="!quizEnd && !quizStart">
      <!-- Status bar: changes as the questionnaire progresses -->
      <div class="statusBar" v-if="currentStatus === 1">
        <p class="statusBarText">
          Intro > <b>Question&emsp14;1 ></b> Question&emsp14;2 >
          Question&emsp14;3 > Question&emsp14;4 > Question&emsp14;5 > Summary
        </p>
      </div>
      <div class="statusBar" v-if="currentStatus === 2">
        <p class="statusBarText">
          Intro > Question&emsp14;1 >
          <b>Question&emsp14;2 ></b> Question&emsp14;3 > Question&emsp14;4 >
          Question&emsp14;5 > Summary
        </p>
      </div>
      <div class="statusBar" v-if="currentStatus === 3">
        <p class="statusBarText">
          Intro > Question&emsp14;1 > Question&emsp14;2 >
          <b> Question&emsp14;3 > </b> Question&emsp14;4 > Question&emsp14;5 >
          Summary
        </p>
      </div>
      <div class="statusBar" v-if="currentStatus === 4">
        <p class="statusBarText">
          Intro > Question&emsp14;1 > Question&emsp14;2 > Question&emsp14;3 >
          <b>Question&emsp14;4 ></b>
          Question&emsp14;5 > Summary
        </p>
      </div>
      <div class="statusBar" v-if="currentStatus === 5">
        <p class="statusBarText">
          Intro > Question&emsp14;1 > Question&emsp14;2 > Question&emsp14;3 >
          Question&emsp14;4 >
          <b>Question&emsp14;5 > </b> Summary
        </p>
      </div>

      <!-- General Question Page content -->
      <div class="generalAlignment">
        <h3 class="header">Question {{ currentQuest }}</h3>
        <p class="text">{{ currentQuestion.text }}</p>
        <div class="radioRectangle">
          <!-- Display each question with their respective answer choices -->
          <p>
            <div 
              v-for="answer in currentQuestion.answers"
              :key="answer"
              >
              <label
                class="text"
                :selected-state="answer === selectedAnswer"
              >
                <input
                  name="choices"
                  id="answer"
                  type="radio"
                  v-model="selectedAnswer"
                  :value="answer"
                />
                  <span class="customRadio"></span>
                  <span class="answerSpan">
                    {{ answer }}
                  </span>
                </label>
              </div>
            </p>
        </div>

      <!-- Two different buttons to display depending on 
      if the quiz is on the final question or not -->
      <button
        class="button"
        v-if="!(currentQuest === quizLength)"
        @click="toNextQuest"
        :disabled="!selectedAnswer"
        :class="{ disabledButton: !selectedAnswer }"
      >
        Next
      </button>
      <button
        class="button"
        v-else
        @click="toNextQuest"
        :disabled="!selectedAnswer"
        :class="{ disabledButton: !selectedAnswer }"
      >
        Finish
      </button>
      </div>
    </div>

    <!--------------------------------------------------------------------->
    <!-- Summary Page ----------------------------------------------------->
    <div v-else>
      <!-- Status bar -->
      <div class="statusBar">
        <p class="statusBarText">
          Intro > Question&emsp14;1 > Question&emsp14;2 > Question&emsp14;3 >
          Question&emsp14;4 > Question&emsp14;5 >
          <b>Summary</b>
        </p>
      </div>
      <!-- Summary Page content -->
      <h2 class="centerHeader">Thank you for completing the questionnaire!</h2>
      <p class="centerTextSummary">
        A summary of your answers and corresponding questions has been detailed
        below.
      </p>
      <!-- Display answers in Summary Box -->
      <div class="summaryBox">
        <div
          v-for="(finalResponse, index) in finalAnswers"
          :key="finalResponse.question"
        >
          <h2 class="header">
            Question {{ index + 1 }}:
            {{ finalResponse.question }}
          </h2>
          <p class="text">You answered:</p>
          <div class="radioRectangleSummary">
            <p>
              <label
                  class="text"
                  selected-state
              >
                  <input
                    id="answer"
                    type="radio"
                    checked
                  />
                  <span class="customRadio"></span>
                  <span class="answerSpan">
                    {{ finalResponse.answer }}
                  </span>
              </label>
            </p>
          </div>
          <br />
          </div>
        </div>
      </div>
    </div>
</template>








<script>
export default {
  props: {
    questions: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      question: this.questions[0],
      quizLength: this.questions.length,
      currentQuest: 0,
      selectedAnswer: "",
      finalAnswers: [],
    };
  },
  methods: {
    toNextQuest() {
      /* Store answers in finalAnswers array for 
      displaying on final page later */
      this.finalAnswers.push({
        question: this.currentQuestion.text,
        answer: this.selectedAnswer,
      });
      /* Advance currentQuest counter to next question and 
      reset selectedAnswer so it can be used in the next iteration */
      this.currentQuest += 1;
      this.selectedAnswer = "";
    },
    startQuiz() {
      this.currentQuest += 1;
    },
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentQuest - 1];
    },
    quizEnd() {
      return this.currentQuest > this.questions.length;
    },
    quizStart() {
      return this.currentQuest === 0;
    },
    currentStatus() {
      return this.currentQuest;
    },
  },
};
</script>








<style scoped>
/* --------------------------------------------------------------------- */
/* Default text/font --------------------------------------------------- */

.title {
  font-family: Open Sans Condensed;
  color: #282828;
  text-align: center;
  margin: 20px;
  margin-top: 40px;
  margin-bottom: 25px;
}
.header {
  font-family: Open Sans Condensed;
  color: #282828;
  margin: 20px;
}
.text {
  font-family: "Segoe UI";
  color: #282828;
  margin: 20px;
}
b {
  color: #FF490C;
}
.statusBarText {
  word-spacing: 20px;
  font-size: 11px;
}
p {
  font-family: Segoe "Segoe UI";
  font-size: 18px;
}

/* --------------------------------------------------------------------- */
/* Special text and specifications ------------------------------------ */

.centerText {
  width: 51%;
  margin-right: auto;
  margin-left: auto;
  text-align: center;
  font-family: Segoe UI, Helvetica;
}
.centerTextSummary {
  width: 60%;
  margin-right: auto;
  margin-left: auto;
  text-align: center;
  font-family: Segoe UI, Helvetica;
  margin-bottom: 80px;
}
.centerHeader {
  font-family: Open Sans Condensed;
  color: #282828;
  margin: 20px;
  text-align: center;
}

/* --------------------------------------------------------------------- */
/* Elements at top of page --------------------------------------------- */

.imageBox {
  background-color: #cb410b;
  height: 140px;
  width: 95%;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 15px;
  margin-top: 30px;
}
.statusBar {
  background-color: #F2F0EF;
  width: 100vw;
  position: relative;
  text-align: center;
  margin-left: -50vw;
  margin-right: -50vw;
  left: 50%;
  right: 50%;
  padding-top: 10px;
  padding-bottom: 15px;
  font-size: 12px;
  font-family: Segoe UI, Helvetica;
  margin-bottom: 40px;
}

/* --------------------------------------------------------------------- */
/* Alignments ---------------------------------------------------------- */

.welcomePage {
  text-align: center;
}
.generalAlignment {
  width: 96%;
  margin-left: auto;
  margin-right: auto;
}

/* --------------------------------------------------------------------- */
/* Configuring box around answer choices ------------------------------- */

/* Define parent class for answer choices section */
.radioRectangle {
  margin-top: 40px;
  margin-bottom: 45px;
  display: relative;
}
/* Box to go around answer choices */
.radioRectangle label {
  position: relative;
  display: block;
  width: 300px;
  background-color: #FFFFFF;
  padding: 12px 20px;
  padding-bottom: 14px;
  border-radius: 6px;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.14);
}

.radioRectangle label:hover {
  cursor: pointer;
  background-color: #EFEDEC;
}

/* --------------------------------------------------------------------- */
/* Custom Radio Button specifications ---------------------------------- */

/* Make default radio button invisible so as to make my own version 
of the radio button */
.radioRectangle input {
  display: none;
}

/* Base element to display my custom radio button */
.customRadio {
  position: absolute;
  top: 17px;
  left: 0;
  height: 11px;
  width: 11px;
  background-color: transparent;
  border-radius: 50%;
  border-style: solid;
  border-width: 1px;
  border-color: #707070;
}

/* When the radio button is checked, add a dark orange/brown background */
label input:checked ~ .customRadio {
  background-color: #776058;
}

/* Create the indicator (the dot/circle - hidden when not checked) */
.customRadio:after {
  content: "";
  position: absolute;
  display: none;
}

/* Display the indicator (dot/circle) */
label input:checked ~ .customRadio:after {
  display: block;
}

/* Style the indicator (dot/circle) */
label .customRadio:after {
  top: 3px;
  left: 3px;
  width: 5px;
  height: 5px;
  border-radius: 50%;
  background: #D9D4D2;
  z-index: 2px;
}

/* When an option is selected, color the box surrounding said option */
.radioRectangle [selected-state] {
  background-color: #D9D4D2;
}

/* --------------------------------------------------------------------- */
/* Controlling span elements ------------------------------------------- */

/* Control placement of custom radio button */
span {
  margin-left: 15px;
}
/* Control answer text placement */
.answerSpan {
  margin-left: 30px;
}

/* --------------------------------------------------------------------- */
/* Button styling ------------------------------------------------------ */

button {
  margin: 20px;
  margin-top: 30px;
  border: none;
  border-radius: 4px;
  background-color: #cb410b;
  color: white;
  height: 40px;
  width: 150px;
  font-size: 16px;
  font-family: "Open Sans Condensed", sans-serif;
  box-shadow: 2px 2px 14px rgba(0, 0, 0, 0.2);
}

.disabledButton {
  cursor: default;
  background-color: #d8d8d8;
}

button:hover:enabled {
  cursor: pointer;
}

/* ----------------------------------------------------------------- */
/* Summary Page visual properties ---------------------------------- */

.summaryBox {
  background-color: #F2F0EF;
  border-radius: 8px;
  padding-top: 30px;
  padding-left: 20px;
  width: 93%;
  margin-left: auto;
  margin-right: auto;
}

/* ----------------------------------------------------------------- */
/* Summary Page styling for radio buttons -------------------------- */

/* Make default radio button invisible */
.radioRectangleSummary input {
  display: none;
}
.radioRectangleSummary {
  margin-top: 0px;
  margin-bottom: 45px;
  display: relative;
}
.radioRectangleSummary label {
  position: relative;
  display: block;
  width: 300px;
  background-color: #FFFFFF;
  padding: 12px 20px;
  padding-bottom: 14px;
  border-radius: 6px;
  box-shadow: 2px 2px 8px rgba(0, 0, 0, 0.14);
}
.radioRectangleSummary label:hover {
  cursor: pointer;
  background-color: #EFEDEC;
}
.radioRectangleSummary [selected-state] {
  background-color: #D9D4D2;
}
</style>