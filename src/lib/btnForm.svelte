<script>
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
    export let details;

    let text = "";
    let fontSize = "";
    let tColor = "";
    let bgColor = "";

    if(details){
        text = details.text;
        fontSize = details.fontSize;
        tColor = details.tColor;
        bgColor = details.bgColor;
    }

    const generate = () => {
        if (text && fontSize && tColor && bgColor && fontSize.match(/^[\d]+$/)){
            dispatch("generate", {text, fontSize, tColor, bgColor});
        }else{
            alert("fill all elements in the form");
        }
    };
</script>
<main>
    <h1>Button generator</h1>
    <div class="form">
        <p>Text:</p>
        <input type="text" bind:value={text}>
        <p>Font size:</p>
        <input type="text" bind:value={fontSize}>
        <div class="colors">
            <section>
                <input type="color" id="color" bind:value={tColor}>
                <p>Text color</p>
            </section>
            <div></div>
            <section>
                <input type="color" id="color" bind:value={bgColor}>
                <p>Background color</p>
            </section>
        </div>
        <button on:click={generate}>Generate</button>
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

.form{
    width: 100%;
    text-align: center;
}

.form>p{
    margin-top: 20px;
    font-size: 1.2rem;
}

.form>input{
    outline: none;
    font-size: .8rem;
    text-align: center;
    padding: 5px 12px;
    border: 2px solid #a4b0be;
    transition: all .2s ease;
}

.form>input:focus{
    padding: 5px 16px;
}

.colors{
    width: 40%;
    text-align: center;
    margin: 40px auto 0px auto;
    display: flex;
    justify-content: space-around;
}

.colors div{
    width: 20px;
}

section{
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}

button{
    margin-top: 20px;
    padding: 6px 10px;
    border: none;
    border-radius: 10px;
    font-size: 1rem;
    background-color: #dfe4ea;
    color: #2f3542;
}

button:hover{
    color: #ffa502;
    cursor: pointer;
}

/*------ Color style ------*/
#color {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    width: 50px;
    height: 50px;
    background-color: transparent;
    border: none;
    cursor: pointer;
  }
  #color::-webkit-color-swatch {
    border-radius: 15px;
    border: none;
  }
  #color::-moz-color-swatch {
    border-radius: 15px;
    border: none;
  }

@media (max-width: 600px) {
    .colors{
        flex-direction: column;
    }
}
</style>