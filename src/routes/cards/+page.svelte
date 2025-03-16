<script lang="ts">
    import {randomInt} from "$lib/utils";
    import {WORDS} from "$lib/words";
    import {BLACK, BLUE, GREY, RED} from "$lib/constants.js";

    let cardWords: string[] = $state(Array(25));
    let cardColorCode: number[] = $state(Array(25));
    let remainingSeconds: number = $state(-1);
    let timerId: number = $state(0);

    const startTimer = (durationSecs: number, output: (seconds: number) => void) => {
        const end = Date.now() + durationSecs * 1000;
        timerId = setInterval(function () {
            const delta = end - Date.now();
            output(Math.floor(delta / 1000));
            if (delta < 0) {
                clearInterval(timerId);
                timerId = 0;
            }
        }, 500);
    }

    const handleTimerButtonClick = () => {
        if (timerId > 0) {
            clearInterval(timerId);
            timerId = 0;
        } else {
            startTimer(60, (seconds: number) => {
                remainingSeconds = seconds < 0 ? 0 : seconds;
            });
        }
    }

    const randomWord = (): string => {
        const usedWords = new Set<number>();
        let z = randomInt(0, WORDS.length - 1);
        while (usedWords.has(z)) {
            z = randomInt(0, WORDS.length - 1);
        }
        usedWords.add(z);
        return WORDS[z];
    };

    function handleClick() {
        if (cardWords[0]) {
            if (!confirm('Generate a new set of cards?')) {
                return;
            }
        }
        cardColorCode = Array(25);
        for (let i = 0; i < 25; ++i) {
            cardWords[i] = randomWord();
        }
    }

    function handleCardColorChange(idx: number) {
        cardColorCode[idx] = ((cardColorCode[idx] || 0) + 1) % 5;
    }
</script>

<div class='page'>
    <div class="cards">
        {#each Array(25) as _,i}
            <div class="card"
                 class:blue={cardColorCode[i] === BLUE}
                 class:red={cardColorCode[i] === RED}
                 class:grey={cardColorCode[i] === GREY}
                 class:black={cardColorCode[i] === BLACK}
            >
                <div class="top">{cardWords[i] || ''}</div>
                <div class="bottom">{cardWords[i] || ''}</div>
                <button class="color" onclick={() => handleCardColorChange(i)} title='Click to switch between colors'>
                    C
                </button>
            </div>
        {/each}
    </div>
    <div class="controls">
        <button class="gen" onclick={handleClick}>Generate Cards</button>

        <div class='timer'>
            <div class:red={remainingSeconds < 10 && remainingSeconds >= 0}>{remainingSeconds < 0 ? '' : remainingSeconds}</div>
            <button onclick={handleTimerButtonClick}>{timerId === 0 ? 'Start' : 'Stop'} Timer</button>
        </div>
    </div>
</div>

<style>
    .page {
        display: flex;
        flex-direction: column;
        height: 100%;
        width: 100%;
    }

    div.cards {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
        grid-gap: 10px;
        height: calc(100% - 4em);
        width: 100%;
        padding: 10px;
        border-radius: 10px;
        border: 1px solid black;
    }


    div.card {
        border: 1px solid black;
        border-radius: 10px;
        background-color: lightyellow;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        position: relative;
        font-size: 1.5em;
    }

    div.card.blue {
        background-color: blue;
        color: transparent;
    }

    div.card.red {
        background-color: red;
        color: transparent;
    }

    div.card.grey {
        background-color: #ccc;
        color: transparent;
    }

    div.card.black {
        background-color: black;
        color: transparent;
    }

    .top {
        opacity: 0.8;
        transform: rotate(180deg);
        padding: 5px;
        margin-bottom: 5px;
        font-style: italic;
    }

    .bottom {
        font-weight: bold;
        padding: 5px;
        margin-top: 5px;
    }

    .card button {
        position: absolute;
        bottom: 3px;
        right: 3px;
        border-radius: 5px;
        border: 1px solid grey;
        width: 2rem;
        height: 2rem;
        background-color: grey;
        color: white;
    }

    .controls {
        display: flex;
        justify-content: space-between;
        margin-top: 0.5em;
        padding: 0;
    }

    .controls button {
        border-radius: 5px;
        padding: 0.5em;
        font-weight: bold;
        width: 10em;
    }

    button.gen {
        background-color: lightgreen;
        border: 1px solid darkgreen;
    }

    button.gen:hover {
        background-color: green;
        color: white;
    }

    .timer {
        display: flex;
        gap: 1em;
    }

    .timer button {
        background-color: lightblue;
        border: 1px solid darkblue;
    }

    .timer button:hover {
        background-color: blue;
        color: white;
    }

    .timer div {
        display: flex;
        align-items: center;
        justify-content: flex-end;
        padding: 0.5em;
        width: 3em;
        border: 1px solid red;
        border-radius: 5px;
        height: 100%;
        font-weight: bold;
    }

    .timer .red {
        background-color: red;
        color: white;
    }
</style>
