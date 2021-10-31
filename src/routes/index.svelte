<script context="module" lang="ts">
	export const prerender = true;
</script>

<script lang="ts">
	import Ruffl from '$lib/components/Ruffl.svelte';
	import Rufflbox from '$lib/components/Rufflbox.svelte';
	const colormap1 = { 1: [0] };
	const colormap2 = { 1: [1,2] };
	const colormap3 = { 1: [2,3,4] };
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<section class="panel headline">
	<Rufflbox 
		firstString = "oiiiooiioi"
		secondString = "frontend"
		gif = "https://c.tenor.com/7Cpec6v3Xc4AAAAd/office-space-bill.gif"
		colormap={colormap1} 
		link="/about"
	/>

	<Rufflbox 
		firstString = "ioioioioii"
		secondString = "backend"
		gif = "https://c.tenor.com/9maoyIgZGNQAAAAd/office-space.gif"
		colormap={colormap1} 
	/>

	<Rufflbox 
		firstString = "iioiooiooi"
		secondString = "weekend"
		gif = "https://c.tenor.com/WyX5TT9_b6sAAAAC/officespace-walking.gif"
		colormap={colormap1} 
	/>
</section>

<section class="">
	<!-- https://c.tenor.com/3fnYUGe0Jh4AAAAd/what-would-you-say-you-do-here-office-space.gif -->
	
	<!-- <div class="title-box">
		<h1>welcome to one ten hundred</h1>
		<h2>(or iioioo, or 110100)</h2>
		<img class="image" src="https://c.tenor.com/_R6JzZGUtZQAAAAd/what-would-you-say-you-do-here-interview.gif" alt="office space gif" />
	</div> -->
	
	<p class="copy">
		welcome to <strong>onetenhundred</strong> (or iioioo, or 110100) 
	</p>
	<p class="copy">
		we specialise in turning ideas into performant, high quality, scalable software <span>(using a mixture of hand drawings, coffee and swearing)</span>
	</p>
	<p class="copy">
		we can build anything <span>(as long as it's interesting <em>(or profitable) <img src="https://c.tenor.com/gE_PWb8KONIAAAAd/office-space-drunk.gif" alt="Office space dancing"/></em>)</span> 
	</p>
	<p class="copy">
		follow the links above to see how we approach things
	</p>
	<p class="copy">
		and <a href="/about">we like chatting</a>, so if you have an idea please <a href="/about">reach out</a>
	</p>
	<p class="copy">
		even if we can't help, we know a ton of <a href="/about">really talented people</a> that may be able to
	</p>
	<p class="copy">
		thanks for stopping by, stay safe, <a href="/about">hein</a>
	</p>

</section>

<style lang="scss">
	
	@import '../lib/design/breakpoints.scss';

	section {
		position: relative; 
		height: 100%;
		width: 100%;

		display: grid;
		align-items: center;
		// padding: 1rem;
		margin: 0 auto;
		box-sizing: border-box;

	}

	section.panel {
		grid-template-columns: repeat(3, 1fr);
	}

	.copy{
		padding: 0;
		margin: 0;
		font-size: 1rem;
    	text-align: center;

		span {
			font-size: 1rem;
			em {
				position: relative;
				font-size: 0.5rem;
				img {
					width: 0.1vw;
					position: absolute;
					top: 50%;
					transform: translateY(50%);
				}
			}
		}
	}
	
	@include respond-to('tablet') { 
		section.panel {
			grid-template-columns: 1fr;
		}

		.copy{
			font-size: 1.4rem;
    		text-align: left;
		}
	}
	
</style>





