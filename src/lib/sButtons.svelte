<script>
    export let details;
    import { onMount } from "svelte";
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    let btn;
    let hBtn;

    onMount(() => {
        createButton(btn);
        createButton(hBtn, true);
    })

    CanvasRenderingContext2D.prototype.roundRect = function (x, y, width, height, radius) {
        if (width < 2 * radius) radius = width / 2;
        if (height < 2 * radius) radius = height / 2;
        this.beginPath();
        this.moveTo(x + radius, y);
        this.arcTo(x + width, y, x + width, y + height, radius);
        this.arcTo(x + width, y + height, x, y + height, radius);
        this.arcTo(x, y + height, x, y, radius);
        this.arcTo(x, y, x + width, y, radius);
        this.closePath();
        return this;
    }

    const createButton = (el, hover) => {
        const canvas = el;
        canvas.width = details.text.length * (0.8 * details.fontSize);
        canvas.height = details.fontSize * 1.5;
        const ctx = canvas.getContext("2d");

        ctx.fillStyle = details.bgColor;
        if (hover){
            ctx.filter = " brightness(50%)";
        }
        ctx.roundRect(0, 0, canvas.width, canvas.height, 20);
        ctx.stroke();
        ctx.fill();
        ctx.fillStyle = details.tColor;
        ctx.font = `bold ${details.fontSize}px Arial`;
        ctx.textAlign = "center";
        ctx.textBaseline = 'middle';
        ctx.fillText(details.text, canvas.width / 2, canvas.height / 2);
    }
</script>
<main>
    <h1>Button generator</h1>
    <div class="buttons">
        <h2 class="buttonsTitle">Main:</h2>
            <canvas bind:this={btn}></canvas>
        <h2 class="buttonsTitle">Hovered:</h2>
            <canvas bind:this={hBtn}></canvas>
        <div class="options">
            <button on:click={() => dispatch("goBack")}>← Go back</button>
            <button on:click={() => dispatch("colorBlind")}>Colorblind test →</button>
        </div>
    </div>
</main>
<style>
main{
    background-color: #2f3542;
    color: #ffa502;
    width: 100svw;
    height: 100svh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
}

main h1{
    margin: 10px 0 10px 0;
    font-size: 3rem;
}

.buttons{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

.buttonsTitle{
    margin-top: 15px;
    text-align: center;
    font-size: 1.7rem;
}

.options{
    padding-top: 10%;
    width: 50vw;
    display: flex;
    align-items: center;
    justify-content: space-around;
}

button{
    border: none;
    background-color: transparent;
    text-decoration: underline;
    font-size: 1rem;
    color: #eccc68;
}

button:hover{
    cursor: pointer;
}
@media (max-width: 600px) {
    .options{
        width: 100svw;
    }
}
</style>