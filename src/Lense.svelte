<script>
	import { fade } from 'svelte/transition';

	export let lense;

	let icon = lense.name.replace(/[\W]+/g, '-');
</script>

<article transition:fade>
	<header>
		<h2>{lense.name}</h2>
		<img src="./../lenses/{icon}.jpg" alt="{lense.name}" />
	</header>
	<section class="lense-specification">
		<p class="lense-focal" title="Focal">{lense.focal}mm</p>
		<p class="lense-max-aperture" title="Max aperture">max f/{lense.aperture.max}</p>
		<p class="lense-min-aperture" title="Min aperture">min f/{lense.aperture.min}</p>
		<p class="lense-weight" title="Weight">{lense.dimensions.weight}g</p>
		<p class="lense-min-focus-distance" title="Minimum Focusing Distance">MFD {lense.focusRange.min/10}cm</p>
	</section>
</article>

<style>
	article {
		text-align: center;
		padding: 1rem;
		max-width: 240px;
		margin: 1rem;
	}

	h2 {
		color: #ed1a3a;
		text-transform: uppercase;
		font-size: 1rem;
		font-weight: 100;
	}

	img {
		width: 100%;
	}

	p {
		margin: 0.2rem 0;
	}

	.lense-specification {
		display: flex;
		flex-flow: row wrap;
		align-items: center;
		font-weight: 100;
	}
	.lense-focal {
		flex-basis: 100%;
	}
	.lense-max-aperture,
	.lense-min-aperture,
	.lense-weight,
	.lense-min-focus-distance {
		flex: 1 1 49%;
	}
	.lense-min-aperture {
		border-left: 1px dotted #4e4e4e;
	}

	@media (min-width: 640px) {
		article {
			max-width: 360px;
		}

		.lense-specification p {
			flex: 1 1 auto;
		}
		.lense-specification p + P {
			border-left: 1px dotted #4e4e4e;
		}
	}
</style>
