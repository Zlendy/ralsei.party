<script lang="ts">
	import Waving from '$lib/assets/ralsei/waving.webp';
	import Shocked from '$lib/assets/ralsei/shocked.webp';
	import Falling from '$lib/assets/ralsei/falling.webp';
	import Splat from '$lib/assets/ralsei/splat.webp';
	import SplatSound from '$lib/assets/ralsei/splat.mp3';

	let y = $state(0);
	let height = $state(0);

	let audio_played = $state(false);

	const image: { src: string; class: string } = $derived.by(() => {
		switch (true) {
			case y < 50:
				return { src: Waving, class: 'w-46' };
			case y < 200:
				return { src: Shocked, class: 'w-54' };
			case y < height - 50:
				return { src: Falling, class: 'w-50' };
			default:
				return { src: Splat, class: 'w-80' };
		}
	});

	$effect(() => {
		if (image.src !== Splat) {
			audio_played = false;
			return;
		}

		if (audio_played) return;
		audio_played = true;

		const audio = new Audio(SplatSound);
		audio.volume = 0.25;
		audio.play();
	});
</script>

<svelte:window bind:scrollY={y} bind:innerHeight={height} />

<img
	class={['rendering-pixelated fixed top-1/2 left-1/2 -translate-1/2', image.class]}
	src={image.src}
	alt="Ralsei"
/>

<main class="h-[200vh]"></main>
