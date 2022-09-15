<script>
  // @ts-ignore
import { onMount } from 'svelte';
  import Stopwatch from './Stopwatch.svelte';
  let stopwatch;

  let numbersInGame = 5;
  const squareCount = 49;
  let gameStarted = false;

  let squareIndexes = [];
  let correctSequence = [];
  let userSequence = [];

  $: times = []; 
  let averageTime;

  function generateSequence() { 
    initialiseSequence(); 
    shuffleSequence();
    correctSequence = sliceSequence(correctSequence);
  }

  function initialiseSequence() {
    squareIndexes = [];
    for (let i = 0; i < squareCount; i++) {
      squareIndexes.push(i);
    }
  }

  function shuffleSequence() {
    // Uses Fisher-Yates algorithm
    correctSequence = squareIndexes;
    for (let i = correctSequence.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      const temp = correctSequence[i];
      correctSequence[i] = correctSequence[j];
      correctSequence[j] = temp;
    }
  }

  function sliceSequence(array) {
    array = array.slice(0, numbersInGame);
    return array;
  }

  function checkPress(squareValue) { 
    userSequence[userSequence.length] = squareValue;
    let lastInputIndex = userSequence.indexOf(squareValue);

    if (squareValue == correctSequence[0]) { 
      startGame();
    } else if (userSequence.length <= 1 && userSequence[0] != correctSequence[0]) {
      userSequence = [];
    } else if (userSequence[lastInputIndex] != correctSequence[lastInputIndex]) {
      resetGame();
    }

    if (gameFinished()) {
      saveTime(); 
      updateAverage();
    }
  }

  function startGame() { 
    gameStarted = true;
    stopwatch.stopStopwatch();
  }

  function resetGame() {
    gameStarted = false;
    stopwatch.resetTimer();
    stopwatch.startStopwatch();
    generateSequence();
    userSequence = [];
  }

  function gameFinished() {
    if (userSequence.length != correctSequence.length) {
      return false;
    }

    for (let i = 0; i < correctSequence.length; i++) {
      if (userSequence[i] != correctSequence[i]) {
        return false;
      }
    }

    return true; 
  }

  function saveTime() {
    times[times.length] = stopwatch.getTime(); 
  }

  function updateAverage() {
    let sum = 0;
    for (let i = 0; i < times.length; i++) {
      sum += times[i]; 
    }
    averageTime = Math.ceil(sum/times.length);
  }

  generateSequence(); 
  onMount(() => {
    stopwatch.startStopwatch();
  });
</script>

<h1> Memory Game </h1>
<!-- Wrapper Start -->
<div class="wrapper">
  <!-- Monkey Game -->
  <div class="monkeyGame">

    <!-- Monkey Grid -->
    <div class="monkeyGrid">
      {#each squareIndexes as square}
          {#if square in correctSequence && !gameStarted}
            <button on:click={() => checkPress(correctSequence[square])}> {square+1} </button>
          {:else if square in userSequence && gameStarted}
            <button> {square+1} </button>
          {:else if square in correctSequence && gameStarted}
            <button on:click={() => checkPress(correctSequence[square])}> ? </button>
          {:else}
            <button class="hiddenButton"></button>
          {/if}
      {/each}
    </div>

    <!-- Game Controls  -->
    <div class="gridControls">
      <label for="numbersInGame">Square Amount:</label>
      <input id="numbersInGame" type="number" step="1" min="1" max="40" bind:value={numbersInGame} on:input={resetGame}/>
      <button on:click={resetGame}>Reset Game</button>
    </div>

  </div>

  <!-- Time Keeping -->
  <div class="timingSection">
    <Stopwatch bind:this={stopwatch} on:load={stopwatch.startStopwatch}/>
      <strong class="averageTime">Average Time: {averageTime} ms</strong>
    <div class="times">

      {#each times as time, i}
        {#if i < 18}
          <p> {time}ms </p>
        {/if}
      {/each}
    </div>
  </div>


</div>

<style>
  h1 {
    color: white;
    /* text-decoration: underline; */
    font-weight: 600;
  }

  .wrapper {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 50px;
  }

  .timingSection {
    /* background-color: rgb(56, 56, 56); */
    min-width: 420px;
    border-radius: 10px;
    margin-top: 0px;
  }

  .averageTime {
    color: white;
    padding: 0;
    margin: 0;
    margin-bottom: 100px;
  }

  .times {
    margin-top: 15px; 
    text-align: left;
    display: grid;
    grid-template-columns: repeat(3, 1fr); 
    grid-gap: 20px; 
    justify-items: center;
    align-items: center;
    text-align: center;
  }

  .times p {
    color: white;
    margin: 0; 
  }

  .monkeyGrid { 
    background-color: #424bf7;
    border-radius: 10px;
    padding: 10px;
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    grid-gap: 10px;
    justify-items: center;
    align-items: center;
    text-align: center;
  }

  .monkeyGrid button {
    text-align: center;
    color: black;
    font-size: 1.7em;
    font-weight: bold;
    background-color: white;
    min-width: 50px;
    min-height: 50px;
    padding: 5px 0px;
  }

  .monkeyGrid button:hover {
    cursor: pointer;
    transform: scale(1.2);
    transition:cubic-bezier(1.2, 0, 0, 1.2);
  }

  .gridControls {
    margin-top: 15px;
  }

  label {
    color: white;
  }

  input[type=number] {
    font-size: 1em;
    padding: 10px;
    margin: 0px 10px;
    border-radius: 20px;
  }

  /* ensures the increment/decrement arrows always display */
  input[type=number]::-webkit-inner-spin-button, 
  input[type=number]::-webkit-outer-spin-button {
      opacity: 1;
  }
</style>
