<script context="module">
	export const contextId = Symbol('community context')
</script>

<script>
	import Button from '$lib/components/Button.svelte'
	import DiscordIcon from '~icons/prime/discord'
	import DiscordProfilePicture from '$lib/components/DiscordProfilePicture.svelte'
	import { setContext } from 'svelte'
	import Title from '$lib/components/Title.svelte'
	import background from '$lib/images/community-bg.webp'
	import amongUsGreenImage from '$lib/images/amongus/green.webp'
	import { discordLink } from '$lib/constants.mjs'

	let sectionElement
	let isDraggingChan = false

	/** @type {{image: string coordinates: [number, number] containerClass: string}[], size: number, quote?: string } */
	const profiles = [
		{
			image: '/imgs/profile_pictures/vaxry.webp',
			coordinates: [187, 296],
			size: 172,
			class: 'outline-red-500'
		},
		{
			image: '/imgs/profile_pictures/fufexan.webp',
			coordinates: [735, 441],
			size: 164,
			class: 'outline-yellow-500'
		},
		{
			image: '/imgs/profile_pictures/raf-notashelf.webp',
			coordinates: [391, 615],
			size: 149,
			class: 'outline-orange-500'
		},
		{
			image: '/imgs/profile_pictures/Mathisbuilder.webp',
			coordinates: [568, 594],
			size: 120,
			class: 'outline-amber-500'
		},
		{
			image: '/imgs/profile_pictures/dani_666..webp',
			coordinates: [525, 764],
			size: 80,
			class: 'outline-red-500'
		},
		{
			image: '/imgs/profile_pictures/end_4.webp',
			coordinates: [648, 709],
			size: 128,
			class: 'outline-cyan-400'
		},
		{
			image: '/imgs/profile_pictures/ardishco.webp',
			coordinates: [65, 208],
			size: 100,
			class: 'outline-slate-200'
		},
		{
			image: '/imgs/profile_pictures/kcrmson.webp',
			coordinates: [53, 399],
			size: 75,
			class: 'outline-sky-500'
		},
		{
			image: '/imgs/profile_pictures/loseardes77.webp',
			coordinates: [24, 341],
			size: 49,
			class: 'outline-green-500'
		},
		{
			image: '/imgs/profile_pictures/captainiveau.webp',
			coordinates: [47, 86],
			size: 48,
			class: 'outline-red-500'
		},
		{
			image: '/imgs/profile_pictures/etrigan63.webp',
			coordinates: [824, 738],
			size: 58,
			class: 'outline-amber-500'
		},
		{
			image: '/imgs/profile_pictures/jsw.webp',
			coordinates: [41, 566],
			size: 49,
			class: 'outline-sky-500'
		},
		{
			image: 'imgs/chan/joy.svg',
			coordinates: [284, 533],
			size: 90,
			class: 'outline-cyan-500 bg-blue-300',
			onDragStart: ({ detail: { currentTarget } }) => {
				isDraggingChan = true
				currentTarget.src = 'imgs/chan/surprise.svg'
			},
			onDragEnd: ({ detail: { currentTarget } }) => {
				isDraggingChan = false
				currentTarget.src = 'imgs/chan/tongueout.svg'
			},
			onHover: ({ detail: { srcElement } }) =>
				!isDraggingChan && (srcElement.src = 'imgs/chan/wink.svg')
		},
		{
			image: '/imgs/profile_pictures/7.webp',
			coordinates: [273, 760],
			size: 52,
			quote: '"meds"',
			class: 'outline-cyan-500'
		},
		{
			image: '/imgs/profile_pictures/outfoxxed.webp',
			coordinates: [648, 364],
			size: 80,
			class: 'outline-orange-500'
		},
		{
			image: '/imgs/profile_pictures/kirottum.webp',
			coordinates: [772, 651],
			size: 62,
			class: 'outline-purple-500'
		},
		{
			image: '/imgs/profile_pictures/beardwarrior.webp',
			coordinates: [736, 277],
			size: 87,
			class: 'outline-amber-500'
		},
		{
			image: '/imgs/profile_pictures/neoney.webp',
			coordinates: [898, 364],
			size: 68,
			class: 'outline-green-500'
		},
		{
			image: amongUsGreenImage,
			coordinates: [873, 224],
			size: 79,
			class: 'outline-green-500'
		},
		{
			image: '/imgs/profile_pictures/SimplyKyle!.webp',
			coordinates: [859, 159],
			size: 39,
			class: 'outline-rose-500'
		},
		{
			image: '/imgs/profile_pictures/sioodmy.webp',
			coordinates: [974, 107],
			size: 48,
			class: 'outline-amber-500'
		},
		{
			image: '/imgs/profile_pictures/flafy.webp',
			coordinates: [147, 553],
			size: 87,
			class: 'outline-pink-500'
		},
		{
			image: '/imgs/profile_pictures/vagahbond.webp',
			coordinates: [65, 643],
			size: 74,
			class: 'outline-amber-500 '
		},
		{
			image: '/imgs/profile_pictures/flick0.webp',
			coordinates: [263, 653],
			size: 65,
			class: 'outline-stone-500 '
		},
		{
			image: '/imgs/profile_pictures/jacekpoz.svg',
			coordinates: [893, 622],
			size: 80,
			class: 'outline-yellow-500 bg-black ',
			quote: '"piss blob"'
		},
		{
			image: '/imgs/profile_pictures/aylur.webp',
			coordinates: [354, 798],
			size: 80,
			class: 'outline-amber-500 bg-black '
		},
		{
			image: '/imgs/profile_pictures/aleph.nought.webp',
			coordinates: [583, 824],
			size: 40,
			class: 'outline-blue-500 bg-black '
		}
	].sort(({ size: a }, { size: b }) => b - a)

	setContext(contextId, {
		biggestSize: profiles.reduce(
			(previousSize, { size }) => (size > previousSize ? size : previousSize),
			1
		),
		smallestSize: profiles.reduce(
			(previousSize, { size }) => (size < previousSize ? size : previousSize),
			Number.POSITIVE_INFINITY
		),
		getSectionElement: () => sectionElement
	})
</script>

<section
	class="relative -mb-[200px] flex h-[1100px] min-h-max w-screen flex-col items-center"
	bind:this={sectionElement}
>
	<Title>
		<span slot="title">Join a great<br />community</span>

		<span slot="subtitle">
			Get help from Distro Hoppers, Haiku writers,<br />Hydrohomies and human_(probably)
		</span>
	</Title>

	<div class="group mt-16 flex flex-col items-center">
		<a
			class="discord p-4"
			href={discordLink}
			target="_blank"
			rel="noopener"
			aria-label="Join us on Discord"
		>
			<DiscordIcon class="h-full w-full  " />
		</a>

		<a href={discordLink}>
			<Button type="fancyOutline">Join us on Discord</Button>
		</a>
	</div>

	<div class="absolute w-[1024px] select-none">
		<div class="flex h-full origin-bottom-right select-none flex-wrap gap-4">
			{#each profiles as { onDragEnd, onDragStart, onHover, ...props }}
				<DiscordProfilePicture
					{...props}
					on:dragStart={onDragStart}
					on:dragEnd={onDragEnd}
					on:hover={onHover}
				/>
			{/each}
		</div>
	</div>
	<img
		src={background}
		class="absolute top-0 -z-10 min-w-[1400px] select-none"
		alt=""
		aria-hidden="true"
		loading="lazy"
	/>
</section>

<style lang="postcss">
	.discord {
		width: 9rem;
		height: 9rem;
		transition:
			rotate 500ms cubic-bezier(0.5, 0, 0.5, 1),
			scale 420ms cubic-bezier(0.5, 0.1, 0, 1),
			filter 840ms;
		transition-delay: 240ms, 180ms, 20ms;
		transform: translateY(-25%);
		filter: drop-shadow(0px 0px 0px cyan) drop-shadow(0px 0px 0px blue);

		&:hover,
		.group:hover & {
			scale: 1.2 1.2;
			rotate: 360deg;
			filter: drop-shadow(4px 4px 14px #0fffef7a) drop-shadow(-4px -4px 12px purple);
			animation: bounce 0.7s infinite 180ms both;
		}
		&:active {
			scale: 1;
			transition: scale 80ms;
		}
	}
</style>
