<script>
    let barCount = 10;
    let bars = [];
    let ith = 0;
    let jth = 1;
    let speed = 100;

    initBars();

    function initBars() {
        bars = [];
        for (let i = 0; i < barCount; ++i) {
            bars.push(Math.floor(Math.random() * 5000) + 1);
        }
        ith = 0;
        jth = 1;
        sort();
    }

    async function sort() {
        let barsCopy = [...bars];
        let reset = false;
        for (let i = 0; i < barsCopy.length - 1; ++i) {
            ith = i;
            for (let j = i + 1; j < barsCopy.length; ++j) {
                jth = j;
                if (barsCopy[i] > barsCopy[j]) {
                    const temp = barsCopy[j];
                    barsCopy[j] = barsCopy[i];
                    barsCopy[i] = temp;
                }
                await new Promise((res) => {
                    setTimeout(() => {
                        if (barsCopy.length !== bars.length) {
                            reset = true;
                        }
                        res(true);
                    }, speed);
                });
                if (reset) break;
                bars = barsCopy;
            }
            if (reset) break;
        }
    }

    function changeBarCount({ target }) {
        barCount = target.valueAsNumber;
        initBars();
    }

    function changeSpeed(radioValue) {
        speed = radioValue;
    }
</script>

<main>
    <div class="bar-wrapper">
        {#each bars as bar, i}
            <div class="bar" class:iselect={i === ith} class:jselect={i === jth} style="height:{bar / 10}px" />
        {/each}
    </div>

    <div class="inputs">
        <label>Number of bars: <input type="number" min="10" max="200" value={barCount} on:change={changeBarCount} /></label>
        <div class="speed">
            Speed
            <label><input type="radio" name="speed" value="1" checked on:change={() => changeSpeed(100)} /> 1</label>
            <label><input type="radio" name="speed" value="2" on:change={() => changeSpeed(50)} /> 2</label>
            <label><input type="radio" name="speed" value="3" on:change={() => changeSpeed(10)} /> 3</label>
        </div>
    </div>
</main>

<style>
    .bar-wrapper {
        display: flex;
        align-items: flex-end;
        width: 100%;
        min-height: 600px;
        justify-content: space-around;
    }
    .bar {
        width: 4px;
        background: red;
    }
    .iselect {
        background: blue;
    }
    .jselect {
        background: green;
    }
    .inputs {
        margin-top: 32px;
    }

    .speed label {
        display: block;
    }
</style>
