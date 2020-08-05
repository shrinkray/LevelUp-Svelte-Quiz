<!-- @format -->

# Svelte For Everyone

### -- Course by: _LevelUp Tutorials_ with _Scott Tolinski_

These are my course notes to keep with the lessons; Not prolific notes, just a few memorable comments each chapter to reference.

## 5. Inputs, Binding and Reactivity

This is a clever way to set a value with an input; no need to set State.

```html
<input bind:value="{title}" type="text" />
```

Adding reactivity is easy with straight forward inputs with `bind:value` and clear results without jargon.

In the lesson we make a simple calculator with numeric input values and adding in a template to display.

## 6. Basic Events

Dive into what makes an event happen

Here created four buttons setting a correct value and returning a response for each. Just beginning, not very dry will get into this in next part.

Differences to Vanila JS in not needing to specify click event. Svelte handles it for us.

## 7. Conditionals and Loops

Clean up quiz btns with a loop using an if-statement to show and hide results

Here the previous code is simplified into a conditional loop using `{#if} {:else} {/if}` templating structure. We learn that a form of promises will be learned later. The loop works well.

## 8. CSS Basics

While in Vue or React, it is common for the `<style></style>` to go below the code, in Svelte, it is more like HTML and is moved upon saving to just below `<script></script>`. It is a nice convention.

- CSS is scoped by default.
- We like things scoped in a components world.
- It's not a Parent > Child relationship
- In Rollup, there's a plugin for global CSS
- There's also a global modifier for CSS.

```html
<style>
  :global(h4) {
    color: tomato;
  }
</style>
```

A component level style will overwrite a global declaration; if itself is global it's the last applied globally. Probably best to be careful about `:global(){...}` assignments. Component-level scoping makes it clean.

Later we'll get into PostCSS/SCSS in Rollup.

## 9. Hitting an API

In this lesson we hit a Trivia Question API and begin to walk the data. Here we are able to see the results pulled called by our function. The lesson ends where the app is not quite returning the correct value.

```js script
// Calling function as a promise initiates a bug

 let quiz = getQuiz();

 ...

async function getQuiz() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=27&difficulty=easy"
    );
    const quiz = await res.json();
    debugger;
  }
```

## 10. Await

Here we will learn to create our own promise by using the await template tag. It will allow us to wait for the result of a promise before rendering our data.

We are waiting for our data ... with Await!

```js
// This is the format, it will be fleshed out in detail next
  {#await promise }

  logging

  {:then data }

  {/await}
```

We await the quiz. As we wait, a Loading message displays. The promise is data. Instead of `quiz.results[0].question` we use `data.results[0].question`. This works super well.

```js script
// This is slick as the promise is being filled, a 'Loading' message shows. After loading our first question appears.
 {#await quiz}
  Loading...
{:then data}
  <h3>{data.results[0].question}</h3>

{/await}
```

New functionality added to enable the app to load the quiz first and make the Get Questions button call the next one. In the next lesson, the interface is cleaned up and functionality is added to the answer inputs.

## 11. Our Questions

We will first refactor and make a Question component for cleaner code. We will then be able to dynamically show a list of all our questions and answer choices.

Create a new component called Question.svelte and set it up to pull answer data from the API.

Some content from the previous Quiz.svelte was removed to enable the rewrite.

```js
// importing our new component

import Question from "./Question.svelte";

...
```

```js
// inside our <div>

{#await quiz}

    Loading...

  {:then data}

    {#each data.results as question}

    // initially this was written as question.question,but Svelte refactors

      <Question {question} />
    {/each}

  {/await}
```

In Question.svelte, we learn how to improve the display of html pulled in from the API by adding **`@html`** prefix.

```js
<script>
  export let question;

  let answers = question.incorrect_answers;
</script>

<h3>
  {@html question.question}
</h3>

{#each answers as answer}
  <button>
    {@html answer}
  </button>
{/each}

```

In React, delivering this code is less clean where we work with Fragments.

## 12. A Working Quiz

Now we will make sure our quiz is working. We will start by showing our correct answer along with our incorrect ones and randomizing the order.

Wow so many new things for this one ...

Commenting added inline on Question.svelte to aid our explanation.

## 13. Interface Improvements

We will add things such as scoring and updating the interface so that only the active question will be shown to the user

**Note: I started over on this install because of an issue with ES6 and maybe ESLint plugin in VSCode at Lesson 13. Am building up work as branches instead. Each branch will be named for the lesson.**

```js
```

Switched some commenting to the components themselves. The page is shaping up but there is a lot more to do to make it look nice.

## 14. Animated Transitions

We will take a look at transitions. Svelte comes with built-in animations that will make it quick and easy to set up transitions between our quiz questions.

This lesson is getting into the visual animation areas, something I enjoy. Transitions are part of Svelte so we import them into our component. The work is happening on the Quiz.svelte component.

```js
// Yeah, these are introduced just like this, missing is the draw transition.
 import { fade, blur, fly, slide, scale } from 'svelte/transition';

...

// A transition is applied as a directive in html. We're dealing with the questions appearing and disappearing. A div wrapper is added to the Question statement. A style adjusts positioning.

<div transition:blur class="fade-wrapper">
    <Question {addToScore} {nextQuestion} {question} />
</div>
```

## 15. Reactive Expressions

### Reactive statements

Adds logic to the application. Place this within the <script></script> area

```js
$: if (score > 1) {
  alert('You won!');
  resetQuiz();
}
```

### Reactive Declarations

A technique to make the code cleaner

```js
$: questionNumber = activeQuestion + 1;
```

## 16. Component Lifecycle

Now we will take a look at lifecycles in Svelte. The lifecycle methods we will cover are onMount, beforeUpdate, afterUpdate and onDestroy.

### onMount

_Run callback when lifecycle run. Only on clientside, (not serverside)._

```js
import { onMount } from "svelte";

...
// onMount Function with Call Back Function that fires when condition met
onMount( () => {
  console.log(`I Mounted!`);
})


```

The next lifecycle items are beforeUpdate, afterUpdate, and onDestroy. The onDestroy function is used _after_ the app is unmounted.

```js
//
import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";

onMount( () => {
  console.log(`I Mounted!`);
})

beforeUpdate( () => {
  console.log(`before update`);
})

afterUpdate( () => {
  console.log(`after update`);
})
...
// the onMount is considered an update, so we see that in the console log.

```

If we click Start a new quiz button, the log would show `before update` and `after update` but not mounted.

## 17. Directive Modifiers & the Class Directive

Next up is Svelte events. We will cover shorthands to make life easier for both the ‘on’ and ‘class’ event directives.

A terrific toolkit for setting html CSS classes through conditional statements.

### Eventname Modifiers. See: [Svelte Docs](https://svelte.dev/docs#class_name)

Use the `on:` directive to listen to DOM events. Make form events `preventDefault` easily without needing to add that in the JS itself.
Join directive modifiers with the (pipe) `|`.

```js
on: eventname = { handler };
on: eventname | modifiers = { handler };
```

In this next example, we have `on:click|once={...}` limiting the restart of the quiz. A use case might be for an ecommerce page where you want to submit an order once.

```html
<button on:click|once="{nextQuestion}">Start New Quiz</button>
```

```html
<form on:submit|preventDefault="{handleSubmit}">
  <!-- the `submit` event's default is prevented,
		 so the page won't reload -->
</form>
```

### Classnames

Using `class:` as a directive, there are many forms to deploy.

```html
...
<!-- These are equivilent  -->
  <h4 class={isCorrect ? 'correct' : 'wrong' } >
    {#if isCorrect}You got it right{:else}You goofed up{/if}
  </h4>

  <h4 class:isCorrect={isCorect} >
    {#if isCorrect}You got it right{:else}You goofed up{/if}
  </h4>
  ...

  <!-- shorthand when the classname is `isCorrect` -->
  <h4 class:isCorrect >
    {#if isCorrect}You got it right{:else}You goofed up{/if}
  </h4>

  <!-- might set multiple classnames -->
  <h4 class:isCorrect class:inactive={!active} >
    {#if isCorrect}You got it right{:else}You goofed up{/if}
  </h4>

```

## 18. Making a Model With Slots

With slots, we have the ability to dynamically insert a component inside another one. We will take our new knowledge of slots to easily make a Modal for the alert box we created earlier.

Added new Modal.svelte component

```html
<!-- Modal.svelte component takes scripts, styles, and elements relating to its style and behaivor -->

<script>
  // transition styles
  import { blur, fade, fly, slide, scale } from 'svelte/transition';
</script>

<style>
  /* modal styles */
</style>

<!-- Inside the div of the Modal, add a <slot /> element -->
<div class="modal-wrapper" transition:fade>
  <div class="modal" transition:scale>
    <button>Close</button>
    <!-- <slot> optional fallback </slot> -->
    <slot />
    <!-- <slot name="modal" /> This did not work! -->
  </div>
</div>

<!-- Hit errors on naming Slots, but there are different ways to target them. -->
```

```js
<!-- In Quiz.svelte -->

<!-- Removing the alert and triggering isModalOpen boolean
isModalOpen becomes true when we get x questions right.
Then it is reset when we restart the quiz
-->
 import Modal from './Modal.svelte';
 let isModalOpen = false;
 ...
```

```html
{#if isModalOpen}
<!--  The Modal element is associated to the Modal component -->
<Modal>
  <h2>You Won!</h2>
  <p>Congratulations!</p>

  <button on:click="{resetQuiz}">Start Over</button>
</Modal>
{/if}
```

## 19. Component Events & Dimensions

```js
```

## 20. CSS & Post Processing

```js
```

## 21. Data Stores & Global State

```js
```

## 22. Sapper & Where To Go From Here

```js
```
