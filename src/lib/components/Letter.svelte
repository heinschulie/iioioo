
<script lang="ts">

    export let backgroundColor = "var(--offWhite)";
    export let letter: SvelteStore<any>;

</script>

<span style="background-color: {backgroundColor};" class="block">
    <h1>{ $letter }</h1>
</span> 

<style lang="scss">

    @import '../design/breakpoints.scss';

    h1 {
        font-size: 10em;
        font-weight: bold;
        width: 100%;
        text-align: center;
        margin: 0; 
    }

</style>

