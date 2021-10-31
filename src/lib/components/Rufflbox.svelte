
<script lang="ts">

    import Ruffl from './Ruffl.svelte'; 

    export let backgroundColor = "var(--offWhite)";
    export let firstString: string;
    export let secondString: string;
    export let link = '';
    
    export let colormap = {}; 
    export let currentColorIndex = 1; 
    export let active = false; 
    export let gif = '';
    export let gifWidth = 1; 

    let showGif = false; 

    let targetArray = firstString.split("");

    const onEnter = () => {
        targetArray = secondString.split("");
        active = true; 
    }

    const onLeave = () => {
        targetArray = firstString.split("");
        active = false; 
        showGif = false;
    }

    function handleMessage(event) {
        console.log("RECEIVED")
		showGif = active;
	}

</script>

<a href={link}>
    <section class:active on:mouseenter={onEnter} on:mouseleave={onLeave}>
        
            {#each targetArray as targetLetter, currentIdx}
            <!-- <span style="background-color: {backgroundColor};" class="block" > -->
                
                <h1>
                    <Ruffl on:message={handleMessage} { targetLetter } { currentIdx } />
                </h1>
                
            <!-- </span> -->
            {/each}

            <!-- {#each Array.of(9 - targetArray.length - 2) as dot}
                <div class="dot"></div>
            {/each} -->

            {#if active}
                <img src={gif} alt="Office space gif" />
            {/if}
        
    </section>
</a>

<style lang="scss">

    @import '../design/breakpoints.scss';

    section {
        display: grid;
        grid-auto-flow: column;
        grid-template-rows: repeat(10, 10vh);
        // grid-template-columns: repeat(3, 33vw);
        position: relative; 

        justify-items: center;
        align-items: center;
    }
    section:hover{
        cursor: pointer;
    }

    @include respond-to('tablet') { 
    
        section {
            display: grid;
            grid-auto-flow: column;
            grid-template-rows: 1fr;
            grid-template-columns: repeat(10, 1fr);
            // gap: 5px;
            position: relative; 

            justify-items: center;
            align-items: center;
        }
        
    }

    // section.active {
    //     background-clip: text;
    //     -webkit-background-clip: text !important;
    //     background-image: url(https://c.tenor.com/5XoBLN7nL0wAAAAC/office-space-meme.gif);
    //     background-size: cover;
    //     background-position: center;
    // }

    section.active {
        background-image: url(https://c.tenor.com/5XoBLN7nL0wAAAAC/office-space-meme.gif);
        background-size: cover;
        background-position: center;
    }

    section.active h1 {
        background-clip: text;
        -webkit-background-clip: text !important;
    }

    h1 {
        font-size: 3em;
        font-weight: bold;
        width: 100%;
        text-align: center;
        margin: 0; 
        color: rgba(0,0,0,0.8);
    }

    .active h1 {
        color: transparent; 
    }
    
    .dot {
        border-radius: 50%;
        width: 15%;
        padding-bottom: 15%;
        background-color: turquoise;
    }
    img {
        width: 100%;
        height: 100%;
        object-fit: cover;
        grid-row: 9 / span 2;
    }

    @include respond-to('tablet') { 

        h1 {
            font-size: 10em;
            font-weight: bold;
            width: 100%;
            text-align: center;
            margin: 0; 
            color: rgba(0,0,0,0.8);
        }
        
        img {
            grid-row: 1;
            grid-column: 9 / span 2;
        }
    }

</style>





