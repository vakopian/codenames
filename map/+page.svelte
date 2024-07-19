<script>
    import {randomInt} from "../utils";

    let frameColorCode = 1;
    const BLUE = 1;
    const RED = 2;
    const GREY = 3;
    const BLACK = 4;
    let cardColorCode = Array(25);

    function randomCardIndex() {
        let z = randomInt(0, 24);
        while (cardColorCode[z]) {
            z = randomInt(0, 24);
        }
        return z;
    }

    function handleClick() {
        cardColorCode = Array(25);
        frameColorCode = randomInt(1, 2);
        for (let i = 0; i < 8; ++i) {
            for (let c of [RED, BLUE]) {
                cardColorCode[randomCardIndex()] = c;
            }
        }
        cardColorCode[randomCardIndex()] = frameColorCode;
        cardColorCode[randomCardIndex()] = BLACK;
    }
</script>

<button on:click={handleClick}>generate map</button>

<div class="frame"
     class:blue={frameColorCode===BLUE}
     class:red={frameColorCode===RED}
>

    {#each Array(25) as _,i}
        <div class="card"
             class:blue={cardColorCode[i] === BLUE}
             class:red={cardColorCode[i] === RED}
             class:grey={cardColorCode[i] === GREY}
             class:black={cardColorCode[i] === BLACK}
        ></div>
    {/each}
</div>

<style>
    button {
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
        height: 300px;
        aspect-ratio: 1/1;
        padding: 10px;
        border-radius: 10px;
    }

    div.frame.blue {
        border: 5px solid blue;
    }

    div.frame.red {
        border: 5px solid red;
    }


    div.card {
        height: 100%;
        aspect-ratio: 1/1;
        border-radius: 10px;
        background-color: #ccc;
    }

    div.card.blue {
        background-color: blue;
    }

    div.card.red {
        background-color: red;
    }

    div.card.grey {
        background-color: #ccc;
    }

    div.card.black {
        background-color: black;
    }
</style>
