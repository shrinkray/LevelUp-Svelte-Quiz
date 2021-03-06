# Svelte For Everyone 
### -- Course by: _LevelUp Tutorials_ with _Scott Tolinski_

These are my course notes to keep with the lessons; Not prolific notes, just a few memorable comments each chapter to reference. 

## 5. Inputs, Binding and Reactivity
  
This is a clever way to set a value with an input; no need to set State. 

 ``` html
  <input bind:value={title} type="text" />
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

``` html

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

``` js script
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

``` js 
// This is the format, it will be fleshed out in detail next
  {#await promise }

  logging
      
  {:then data } 
      
  {/await}
  ```

We await the quiz. As we wait, a Loading message displays. The promise is data. Instead of `quiz.results[0].question` we use `data.results[0].question`. This works super well. 

  ``` js script
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

``` js
// importing our new component

import Question from "./Question.svelte";

...
```
``` js 
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

``` js
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

``` js

```

## 14. Animated Transitions


## 15. Reactive Expressions


## 16. Component Lifecycle


## 17. Directive Modifiers & the Class Directive


## 18. Making a Model With Slots


## 19. Component Events & Dimensions


## 20. CSS & Post Processing


## 21. Data Stores & Global State


## 22. Sapper & Where To Go From Here
