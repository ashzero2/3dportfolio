<script lang="ts">
	import Bounded from '$lib/components/Bounded.svelte';
	import Heading from '$lib/components/Heading.svelte';
	import type { Content } from '@prismicio/client';
	import { gsap } from 'gsap';
	import scrollTrigger from 'gsap/dist/ScrollTrigger';
	import { onMount } from 'svelte';
	import IconCircle from '~icons/ic/baseline-circle';

	export let slice: Content.TechListSlice;
	gsap.registerPlugin(scrollTrigger);

	let component: HTMLElement;

	onMount(() => {
		const tl = gsap.timeline({
			scrollTrigger: {
				trigger: component,
				start: 'top bottom',
				end: 'bottom top',
				scrub: 4
			}
		});

		tl.fromTo(
			'.tech-row',
			{
				x: (index) => {
					return index % 2 === 0 ? gsap.utils.random(600, 400) : gsap.utils.random(-600, -400);
				}
			},
			{
				x: (index) => {
					return index % 2 === 0 ? gsap.utils.random(-600, -400) : gsap.utils.random(600, 400);
				},
				ease: 'power1.inOut'
			}
		);
	});
</script>

<section
	data-slice-type={slice.slice_type}
	data-slice-variation={slice.variation}
	class="overflow-hidden"
	bind:this={component}
>
	<Bounded as="div">
		<Heading size="xl" class="mb-8" tag="h2">
			{slice.primary.heading}
		</Heading>
	</Bounded>

	{#each slice.primary.item as { tech_name, tech_color }}
		<div class="tech-row mb-8 flex items-center justify-center gap-4 text-slate-700">
			{#each Array(15) as _, index}
				<span
					class="tech-item text-8xl font-extrabold uppercase tracking-tighter"
					style="color: {index == 7 && tech_color ? tech_color : 'inherit'};"
				>
					{tech_name}
				</span>
				<span class="text-3xl">
					<IconCircle></IconCircle>
				</span>
			{/each}
		</div>
	{/each}
</section>
