<script>
	import LL from '$i18n/i18n-svelte'
	import { crossfade, fade, fly, slide, blur } from 'svelte/transition'
	import { quintOut } from 'svelte/easing'

	// export let id, title, desc, book, src;
	export let id, number

	const [send, receive] = crossfade({
		duration: (d) => Math.sqrt(d * 100),

		fallback(node, params) {
			const style = getComputedStyle(node)
			const transform = style.transform === 'none' ? '' : style.transform

			return {
				duration: 300,
				easing: quintOut,
				css: (t) => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`,
			}
		},
	})
</script>

{#key id}
	<div out:fade={{ duration: 0 }} in:slide>
		<div class="card-course">
			<img
				loading="lazy"
				src={`/books/${id}.jpg`}
				onerror="this.onerror=null; this.src='/books/fallback.jpg'"
				alt={$LL.courses[id].book()}
			/>
			<div class="card-inner">
				<h3>{$LL.courses[id].book()}</h3>
				<subtitle>{$LL.courses[id].title()}</subtitle>
				<p>{$LL.courses[id].desc()}</p>
			</div>
		</div>
	</div>
{/key}

<style>
	subtitle {
		margin: 0 0 5px 0;
	}
	h3,
	p {
		margin: 0;
	}
	div.card-course {
		display: flex;
		flex-direction: row;
		align-items: center;
		border-radius: 8px;
		background-color: var(--pure-white);
		/* min-width: 200px; */
		/* flex: 2 1 calc(25% - 1em); */
		padding: 10px 0px;
		margin: 10px;
		max-width: 350px;
		min-width: 300px;
		transition: top ease 200ms;
		transition: box-shadow ease 200ms;
	}
	div.card-course:hover > img {
		box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
		transform: translateY(-2px);
		transition: all 0.1s ease-in-out;
		top: -10px;
	}
	div.card-inner {
		display: flex;
		flex-direction: column;
		margin: 0 5px;
	}
	img {
		/* height: auto; */
		/* width: auto; */
		/* max-width: 100px; */
		max-height: 163px;
		border-radius: 8px;
		margin: 0px 10px;
	}
	a {
		color: var(--text-color);
	}
	subtitle {
		font-weight: 400;
		font-weight: lighter;
	}
</style>
