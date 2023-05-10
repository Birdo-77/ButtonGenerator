<script>
    import { Blind } from "color-blind/lib/blind";
    import { onMount } from "svelte";
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    const titles = ["Original", "Protanopia", "Protanomaly", "Tritanopia"]
    let hBtn = [undefined, undefined, undefined, undefined];
    let Btn = [undefined, undefined, undefined, undefined];
    export let details;

    // TODO: Gerenerate buttons in the color-blinded version
    // Create a separated file for the function createButtons 
    // and use in here

    const rgbToHex = (r, g, b) => '#' + [r, g, b].map(x => {
        const hex = x.toString(16)
        return hex.length === 1 ? '0' + hex : hex
    }).join('')

    function rgb(hex) {
        const result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
        return {
            R: parseInt(result[1], 16),
            G: parseInt(result[2], 16),
            B: parseInt(result[3], 16)
        }
    }

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

    const createButton = (el, i, hover) => {
        let _bgColor = details.bgColor;
        if (i != 0) {
            const _b = Blind(
                rgb(details.bgColor), 
                titles[i].substring(0, 6).toLowerCase(), 
                i == 2
            );
            _b.R = Number(_b.R.toFixed(0));
            _b.G = Number(_b.G.toFixed(0));
            _b.B = Number(_b.B.toFixed(0));

            _bgColor = rgbToHex(_b.R, _b.G, _b.B);

            console.log(_bgColor);
        }
        const canvas = el;
        canvas.width = details.text.length * (0.8 * details.fontSize);
        canvas.height = details.fontSize * 1.5;
        const ctx = canvas.getContext("2d");

        ctx.fillStyle = _bgColor;
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

    onMount(() => {
        Btn.forEach((el, i) => {
            createButton(el, i);
        });
        hBtn.forEach((el, i) => {
            createButton(el, i, true);
        });
    })

    const download = () => {
        let canvas = Btn[0];
        const anchor = document.createElement("a");
        anchor.href = canvas.toDataURL("image/png");
        anchor.download = "btn.png";
        anchor.click();
        canvas = hBtn[0];
        anchor.href = canvas.toDataURL("image/png");
        anchor.download = "hBtn.png";
        anchor.click();
    }
</script>
<main>
    <h1>Button generator</h1>
    <div class="viewer">
        <div class="mainBtn">
            {#each Btn as b, i}
                <section>
                    <h2>{ titles[i] }</h2>
                    <canvas bind:this={b}></canvas>
                </section>
            {/each}
        </div>
        <div class="hoverBtn">
            {#each hBtn as b, i}
                <section>
                    <h2>{ titles[i] }</h2>
                    <canvas bind:this={b}></canvas>
                </section>
            {/each}
        </div>
    </div>
    <div class="options">
        <button on:click={() => dispatch("goBack")}>← Go back</button>
        <!-- Download the buttons -->
        <button on:click={download}>Download →</button>
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
.mainBtn, .hoverBtn{
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.mainBtn{
    margin-bottom: 20px;
}

.mainBtn section:nth-child(1), .hoverBtn section:nth-child(1){
    margin-right: 40px;
}

.mainBtn section, .hoverBtn section{
    margin-left: 40px;
    text-align: center;
}

.viewer{
    height: 60%;
    display: flex;
    align-items: center;
    flex-direction: column;
    justify-content: center;
}

.options{
    padding-top: 20px;
    width: 50vw;
    display: flex;
    align-items: center;
    justify-content: space-around;
}

h2{
    margin-bottom: 5px;
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
    .mainBtn section:nth-child(1), .hoverBtn section:nth-child(1){
        margin-right: 0px;
    }
    .mainBtn section, .hoverBtn section{
        margin-left: 0px;
    }
    main{
        width: auto;
        height: auto;
    }
    .mainBtn, .hoverBtn{
        flex-direction: column;
    }
    .options{
        width: 100%;
        margin-bottom: 20px;
    }
}
</style>