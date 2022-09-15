# Chimp Memory Game

## Overview
With Svelte, HTML/CSS/JavaScript I managed to implement a playable version of the famous chimp memory. The game goes as follows: 
1. View all of the squares, remembering the numbers on them
2. Click the first square, the rest of the squares numbers will be hidden
3. Click the remaining squares in the correct order to reveal them

## How the Timer Works
There is also a timer which starts when the board is reset, and stops when you interact with the first square. If you succesfully complete the game, 
a time is added to the board and an average time is calculated. This is to demonstrate how long it takes to memorise them. In the experiment, chimps
have a remarkably short term memory and almost instantaneously memorise the correct order for 9 squares.  

## Future Update Plans
Svelte, and regular JavaScript were used to implement this project. I'm going to try and improve this project by making it more responsive so it can be played on phones, abd breaking down App.svelte into separate components so it's more readable. 