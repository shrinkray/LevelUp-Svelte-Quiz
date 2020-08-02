<script>
  export let question;
  export let nextQuestion;

  let isCorrect;
  let isAnswered = false;

  // the Map method makes a new array from an old array. It does not change the old array.
  // Here it is taking in the incorrect answers from the API and returning an object of the string plus a correct key.
  let answers = question.incorrect_answers.map((answer) => {
    return {
      answer,
      correct: false,
    };
  });

  // Collects all answer values into an array. We're using spread as a quick method
  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true,
    },
  ];
  shuffle(allAnswers);

  // Shuffle is pretty cool. This trick assigns and sorts for a number between -1 and 1
  function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
  }

  // When the correct answer is clicked, assign isAnswered and isCorrect values
  function checkQuestion(correct) {
    isAnswered = true;
    isCorrect = correct;
  }
</script>

<!-- Presentation of the question from the API -->
<h3>
  {@html question.question}
</h3>

<!-- Presents response aligned with correct or incorrect answer -->
{#if isAnswered}
  <h4>
    {#if isCorrect}You got it right{:else}You goofed up{/if}
  </h4>
{/if}

<!-- Displays answers as a button. The second item is a cheat for testing false and correct answers. Comment out to keep us honest. -->

{#each allAnswers as answer}
  <button on:click={() => checkQuestion(answer.correct)}>
    {@html answer.answer}
    {@html answer.correct}
  </button>
{/each}
{#if isCorrect}
  <div>
    <button on:click={nextQuestion}>Next Question</button>
  </div>
{/if}
