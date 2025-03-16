<script lang="ts">
    import {randomInt} from "$lib/utils";
    import {BLACK, BLUE, GREY, RED} from "$lib/constants.js";

    let frameColorCode: number = $state(0);
    let cardsColorCodes: number[] = $state(Array(25));

    const randomCardIndex = () => {
        let z = randomInt(0, 24);
        while (cardsColorCodes[z]) {
            z = randomInt(0, 24);
        }
        return z;
    }

    const handleClick = () => {
        if (frameColorCode) {
            if (!confirm('Generate a new map?')) {
                return;
            }
        }
        cardsColorCodes = Array(25);
        frameColorCode = randomInt(1, 2);
        for (let i = 0; i < 8; ++i) {
            for (let colorCode of [RED, BLUE]) {
                cardsColorCodes[randomCardIndex()] = colorCode;
            }
        }
        cardsColorCodes[randomCardIndex()] = frameColorCode;
        cardsColorCodes[randomCardIndex()] = BLACK;
    }
</script>

<div class='page'>
    <div class="cards" class:blue={frameColorCode === BLUE} class:red={frameColorCode === RED}
         class:grey={frameColorCode === 0}>
        {#each cardsColorCodes as cardColorCode}
            <div class="card"
                 class:blue={cardColorCode === BLUE}
                 class:red={cardColorCode === RED}
                 class:grey={cardColorCode === GREY}
                 class:black={cardColorCode === BLACK}
            ></div>
        {/each}
    </div>

    <div class='controls'>
        <button class="gen" onclick={handleClick}>Generate Map</button>
    </div>
</div>


<style>
    .controls {
        display: flex;
        justify-content: space-between;
        margin-top: 0.5em;
        padding: 0;
    }

    button.gen {
        border-radius: 5px;
        padding: 0.5em;
        font-weight: bold;
        width: 10em;
        background-color: lightgreen;
        border: 1px solid darkgreen;
    }

    button.gen:hover {
        background-color: green;
        color: white;
    }

    div.cards {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
        grid-gap: 10px;
        height: 300px;
        aspect-ratio: 1/1;
        padding: 10px;
        border-radius: 10px;
    }

    div.cards.grey {
        border: 5px solid #ccc;
    }

    div.cards.blue {
        border: 5px solid blue;
    }

    div.cards.red {
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
