<script>
  // This was my first component in Svelte
  // Here getting an awareness of the templating varables and how a value set with let vs const
  // allows an exported variable to be over written in another file.

  import Question from "./Question.svelte";

  let quiz = getQuiz();
  let activeQuestion = 0;
  let score = 0;

  async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=27&difficulty=hard"
    );
    const quiz = await res.json();
    return quiz;
  }

  function handleClick() {
    quiz = getQuiz();
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }
  function resetScore() {
    score = 0;
  }
  function addToScore() {
    score = score + 1;
  }
</script>

<div>

  <button on:click={handleClick}>Start New Quiz</button>

  <h3>My Score: {score}</h3>
  <h4>Question #{activeQuestion + 1}</h4>

  {#await quiz}
    Loading...
  {:then data}

    {#each data.results as question, index}
      {#if index === activeQuestion}
        <Question {addToScore} {nextQuestion} {question} />
      {/if}
    {/each}

  {/await}

</div>
