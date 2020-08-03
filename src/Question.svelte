<script>
  export let question;
  export let nextQuestion;
  export let addToScore;

  let isCorrect;
  let isAnswered = false;

  // Iterates over the items in an array and creates a new array.  It does not change the old array.
  // Here it is taking in the incorrect answers from the API and returning an array of the incorrect answers.
  let answers = question.incorrect_answers.map((answer) => {
    return {
      answer,
      correct: false,
    };
  });

  // Appends the incorrect answers to our correct answer.
  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true,
    },
  ];
  shuffle(allAnswers);

  // Shuffle is pretty cool. Not the most random, but good enough for what this is. This assigns and sorts for a number between 0 to 1 by subtracting 0.5 from it, our result range is: -0.5 and 0.5.
  function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
  }

  // When the correct answer is clicked, assign isAnswered and isCorrect values
  function checkQuestion(correct) {
    if (!isAnswered) {
      isAnswered = true;
      isCorrect = correct;

      if (correct) {
        addToScore();
      }
    }
  }
</script>

<!-- Presentation of the question from the API -->
<h3>
  {@html question.question}
</h3>

<!-- Presents response aligned with correct or incorrect answer 
 When the answer is correct, the next quesiton button appears. 
  The templating framework is more explicit. We could use a terary but this form is more explicit. 
-->
{#if isAnswered}
  <h4>
    {#if isCorrect}You got it right{:else}You goofed up{/if}
  </h4>
{/if}

<!-- Displays answers as a button. The second item is a cheat for testing false and correct answers. Comment out to keep us honest. 

We'e running an arrow function if the question is correct. And created a new function called correctQuestion() where if correct something fun will happen. 
-->

{#each allAnswers as answer}
  <button disabled={isAnswered} on:click={() => checkQuestion(answer.correct)}>
    {@html answer.answer}
    {@html answer.correct}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}>Next Question</button>
  </div>
{/if}
