<script>
	import {
		BehaviorSubject,
		Subject,
		combineLatest,
		debounceTime,
		delay,
		distinctUntilChanged,
		filter,
		map,
		of,
		startWith,
		switchMap,
		timer
	} from 'rxjs'
	import { onDestroy, onMount } from 'svelte'
	import { spring } from 'svelte/motion'

	/** The start position of the gradient. */
	export let startPosition = [-1000, -1000]

	/** @type {HTMLDivElement}*/
	let wrapperElement = undefined
	/** @type {import('rxjs').BehaviorSubject<boolean>}*/
	const isMouseOver$ = new BehaviorSubject(false).pipe(
		// Do not harshly stop updating the gradient when the mouse leaves, but wait a bit
		switchMap((isTrue) => (isTrue ? of(isTrue) : timer(1500).pipe(map(() => false)))),
		distinctUntilChanged()
	)

	/** @type {import('rxjs').BehaviorSubject<number>}*/
	const gradientSize$ = new BehaviorSubject().pipe(
		// Debounce resize events with some high number for performance
		debounceTime(1),
		map(() => wrapperElement.getBoundingClientRect().width * 3),
		startWith(800)
	)
	/** @type {import('rxjs').Subject< {clientX: number, clientY: number} >}*/
	const mousePosition$ = new Subject()

	const gradientPosition$ = combineLatest([mousePosition$, gradientSize$, isMouseOver$]).pipe(
		filter(([_, __, isMouseOver]) => isMouseOver),
		map(([{ clientX, clientY }, gradientSize]) => {
			const { x, y } = wrapperElement?.getBoundingClientRect() ?? { x: 0, y: 0 }
			return [clientX - x - gradientSize * 0.5, clientY - y - gradientSize * 0.5]
		}),
		startWith(startPosition)
	)
	const gradientWiggle = spring(startPosition, { damping: 0.95, stiffness: 0.1 })
	const subscription = gradientPosition$.subscribe((data) => gradientWiggle.set(data))

	onDestroy(() => {
		subscription.unsubscribe()
	})

	let hasJustMounted = true
	onMount(() => {
		resizeGradient()

		hasJustMounted = false
	})

	$: {
		if (!$isMouseOver$) {
			globalThis.document?.removeEventListener('mousemove', track)
		}
	}

	function resizeGradient() {
		if (hasJustMounted || !isMouseOver$) return

		gradientSize$.next()
	}

	function startTrackingMouse() {
		if ($isMouseOver$) return

		globalThis.document?.addEventListener('mousemove', track)
	}

	function track({ clientX, clientY }) {
		mousePosition$.next({ clientX, clientY })
	}

	onDestroy(() => {
		globalThis.document?.removeEventListener('mousemove', track)
	})
</script>

<svelte:window on:resize={resizeGradient} />

<div
	class={$$props.class + '  wrapper'}
	on:mouseenter={startTrackingMouse}
	on:mouseleave={({ clientX, clientY, currentTarget }) => {
		const { x, width, y, height } = currentTarget.getBoundingClientRect()
		const isMouseStillOver =
			x <= clientX && y <= clientY && x + width > clientX && y + height > clientY
		isMouseOver$.next(isMouseStillOver)
	}}
	aria-hidden
	bind:this={wrapperElement}
>
	<div
		class="gradient"
		style:--x={$gradientWiggle.at(0) + 'px'}
		style:--y={$gradientWiggle.at(1) + 'px'}
		style:--size={$gradientSize$ + 'px'}
	></div>

	<svg width="100%" height="100%" fill="none" xmlns="http://www.w3.org/2000/svg">
		<pattern
			id="background-pattern-id"
			x="0"
			y="0"
			width="30"
			height="30"
			patternUnits="userSpaceOnUse"
		>
			<rect x="0.5" y="0.5" width="30" height="30" rx="0" stroke="currentColor" />
		</pattern>

		<rect
			x="0"
			y="0"
			width="100%"
			height="100%"
			filter="url(#spotlight)"
			fill="url(#background-pattern-id)"
		>
		</rect>
	</svg>
</div>

<style lang="postcss">
	.wrapper {
		mask-image: linear-gradient(black 75%, transparent);
		contain: strict;
		user-select: none;
	}
	svg {
		background: theme(colors.black);
	}

	.gradient {
		position: absolute;
		top: 0;
		left: 0;
		mix-blend-mode: color-dodge;
		height: var(--size);
		width: var(--size);
		background: radial-gradient(
			closest-side,
			theme(colors.cyan.300),
			theme(colors.blue.950 / 100%) 30%,
			transparent
		);
		opacity: 100%;
		translate: var(--x) var(--y);
		z-index: 20;
	}
</style>
