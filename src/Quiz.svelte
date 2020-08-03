<script>
  // This was my first component in Svelte
  // Here getting an awareness of the templating varables and how a value set with let vs const
  // allows an exported variable to be over written in another file.

  import { fade, blur, fly, slide, scale } from 'svelte/transition';
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from 'svelte';

  import Question from './Question.svelte';

  let quiz = getQuiz();
  let activeQuestion = 0;
  let score = 0;

  async function getQuiz() {
    const res = await fetch(
      'https://opentdb.com/api.php?amount=10&category=27&difficulty=hard'
    );
    const quiz = await res.json();
    return quiz;
  }

  // Function to advance to the next question
  // Scott's thought is that this form is more readable than ...
  // activeQueston +=

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }
  function resetQuiz() {
    score = 0;
    activeQuestion = 0;
    quiz = getQuiz();
  }
  function addToScore() {
    score = score + 1;
  }
  // Reactive Statement
  $: if (score > 7) {
    alert('You won!');
    resetQuiz();
  }

  onMount(() => {
    console.log(`I Mounted!`);
  });

  beforeUpdate(() => {
    console.log(`before update`);
  });

  afterUpdate(() => {
    console.log(`after update`);
  });

  // Reactive Declartation
  $: questionNumber = activeQuestion + 1;
</script>

<style>
  .fade-wrapper {
    position: absolute;
  }
</style>

<!-- INTERFACE OF THE PAGE 
  * Main Quiz Button
  * Score and a basic question number.
-->
<div>

  <button on:click={nextQuestion}>Start New Quiz</button>

  <h3>My Score: {score}</h3>
  <h4>Question #{questionNumber}</h4>

  <!-- This is cool, it awaits until the info is loaded then show the data. Nice.  -->
  {#await quiz}
    Loading...
  {:then data}

    <!-- Adding new index ( it is not a keyword and could be anything, like a letter or other word. Index is the first parameter.  ) 
  
    This if section makes it so we only show one question at a time. 
  -->

    {#each data.results as question, index}
      {#if index === activeQuestion}
        <!-- This is an example of passing a function down into our Question.svelte component -->

        <div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
          <Question {addToScore} {nextQuestion} {question} />
        </div>
      {/if}
    {/each}

  {/await}

</div>
