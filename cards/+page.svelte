<script>
    import {WORDS} from "./words.js";
    import {randomInt} from "../utils";

    const BLUE = 1;
    const RED = 2;
    const GREY = 3;
    const BLACK = 4;

    let cardWords = Array(25);
    let cardColorCode = Array(25);
    const usedWords = new Set();
    let timer = -1;

    const startTimer = (durationSecs, output) => {
        const end = Date.now() + durationSecs * 1000;
        const id = setInterval(function () {
            const delta = end - Date.now();
            output(Math.floor(delta / 1000));
            if (delta < 0) {
                clearInterval(id);
            }
        }, 500);
    }

    const handleStarTimer = () => {
        startTimer(60, (s) => {
            timer = s < 0 ? 0 : s;
        });
    }

    function randomWord() {
        let z = randomInt(0, WORDS.length - 1);
        while (usedWords.has(z)) {
            z = randomInt(0, WORDS.length - 1);
        }
        usedWords.add(z);
        return WORDS[z];
    }

    function handleClick() {
        cardColorCode = Array(25);
        for (let i = 0; i < 25; ++i) {
            cardWords[i] = randomWord();
        }
    }

    function handleCardColorChange(idx) {
        cardColorCode[idx] = ((cardColorCode[idx] || 0) + 1) % 5;
    }
</script>

<button class="gen" on:click={handleClick}>generate cards</button>

<div class='timer'>
    <button on:click={handleStarTimer}>Start Timer</button>
    <div class:red={timer < 10 && timer >= 0}>{timer < 0 ? '' : timer}</div>
</div>

<div class="frame">

    {#each Array(25) as _,i}
        <div class="card"
             class:blue={cardColorCode[i] === BLUE}
             class:red={cardColorCode[i] === RED}
             class:grey={cardColorCode[i] === GREY}
             class:black={cardColorCode[i] === BLACK}
        >
            <div class="top">{cardWords[i] || ''}</div>
            <div class="bottom">{cardWords[i] || ''}</div>
            <button class="color" on:click={() => handleCardColorChange(i)}>C</button>
        </div>
    {/each}
</div>

<style>
    button.gen {
        position: absolute;
        top: 10px;
        right: 10px;
        padding: 10px;
        height: auto;
    }

    div.frame {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
        grid-gap: 10px;
        height: 600px;
        width: 1060px;;
        padding: 10px;
        border-radius: 10px;
        border: 1px solid black;
    }


    div.card {
        height: 100px;
        width: 200px;
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
        width: 2em;
        height: 2em;
        background-color: grey;
        color: white;
    }

    .timer {
        position: absolute;
        top: 100px;
        right: 10px;
    }

    .timer div {
        display: flex;
        align-items: center;
        justify-content: flex-end;
        margin: 5px 0;
        padding: 5px;
        width: 5em;
        border: 1px solid #ccc;
        border-radius: 5px;
        font-weight: bold;
        height: 2em;
    }

    .timer .red {
        background-color: red;
        color: white;
    }
</style>
