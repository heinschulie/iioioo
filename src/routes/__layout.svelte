<script lang="ts">
	import Header from '$lib/header/Header.svelte';
	import '../app.css';
</script>

<main>
	<slot />
</main>

<style>
	main {
		/* display: grid;
		grid-auto-rows: 33%;
		padding: 1rem;
		width: 100%;
		height: 100vh;
		margin: 0 auto;
		box-sizing: border-box; */


		/* display: grid;
		grid-auto-rows: 100vh; */
		height: 100vh;
	}
/* 
	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 40px;
	}

	footer a {
		font-weight: bold;
	} */

	/* @media (min-width: 480px) {
		footer {
			padding: 40px 0;
		}
	} */
</style>
