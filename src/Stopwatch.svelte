<script>
    // Times
    let totalMilliSeconds = 0;

    // Pause Functionality
    let startTime = Date.now();
    let stopwatchState;

    export function getTime() {
        return totalMilliSeconds;
    }

    export function startStopwatch() {
        startTime = Date.now();
        stopwatchState = setInterval(incrementTimer, 50);
    }

    function incrementTimer() {
        let currentTime = Date.now() - startTime;
        totalMilliSeconds = Math.floor(currentTime);
    }

    export function stopStopwatch() {
        clearInterval(stopwatchState);
    }

    export function resetTimer() {
        totalMilliSeconds = 0;
        stopStopwatch();
    }

    const isOneDigit = (number) => number.toString().length === 1;
</script>

<div class="stopwatch">
    ⏱{totalMilliSeconds}ms
</div>

<style>
    .stopwatch {
        background-color: black;
        color: white;
        justify-content: center;
        font-size: 2em;
        font-weight: bold;
        padding: 20px;
        margin: 10px;
        border: black 10px solid;
        border-radius: 10px;
        display: flex;
        align-items: center;
    }
</style>
