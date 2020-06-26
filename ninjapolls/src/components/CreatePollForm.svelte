<script>
  import PollStore from "../stores/PollStore.js";
  import Button from "../shared/Button.svelte";
  import { createEventDispatcher } from "svelte";
  const dispatch = new createEventDispatcher();

  let fields = { question: "", answerA: "", answerB: "" };
  let errors = { question: "", answerA: "", answerB: "" };
  let valid = false;

  const submitHandler = () => {
    valid = true;

    // Validate Question
    if (fields.question.trim().length < 5) {
      valid = false;
      errors.question = "Question must be at least 5 letters long.";
    } else {
      errors.question = "";
    }

    // Validate Answer A
    if (fields.answerA.trim().length < 1) {
      valid = false;
      errors.answerA = "Answer A must not be empty.";
    } else {
      errors.answerA = "";
    }

    // Validate Answer B
    if (fields.answerB.trim().length < 1) {
      valid = false;
      errors.answerB = "Answer B must not be empty.";
    } else {
      errors.answerB = "";
    }

    // Add new poll
    if (valid) {
      let poll = { ...fields, votesA: 0, votesB: 0, id: Math.random() };
      PollStore.update(currentPolls => {
        return [poll, ...currentPolls];
      });
      dispatch("add");
    }
  };
</script>

<style>
  form {
    width: 400px;
    margin: 0 auto;
    text-align: center;
  }
  .form-field {
    margin: 18px auto;
  }
  input {
    width: 100%;
    border-radius: 6px;
  }
  label {
    margin: 10px auto;
    text-align: left;
  }
  .errors {
    font-weight: bold;
    color: #d91b42;
    font-size: 12px;
  }
</style>

<form on:submit|preventDefault={submitHandler}>
  <div class="form-field">
    <label for="question">Poll Question:</label>
    <input id="question" type="text" bind:value={fields.question} />
    <div class="errors">{errors.question}</div>
  </div>
  <div class="form-field">
    <label for="answer-a">Answer A:</label>
    <input id="question" type="answer-a" bind:value={fields.answerA} />
    <div class="errors">{errors.answerA}</div>
  </div>
  <div class="form-field">
    <label for="answer-b">Answer B:</label>
    <input id="question" type="answer-b" bind:value={fields.answerB} />
    <div class="errors">{errors.answerB}</div>
  </div>
  <Button type="secondary" flat={true}>Add Poll</Button>
</form>
