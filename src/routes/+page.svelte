<script lang="ts">
	import HoldButton from '$lib/components/HoldButton.svelte';
	import MemoryModal from '$lib/components/MemoryModal.svelte';
	import FlowerBloom from '$lib/components/svg/FlowerBloom.svelte';
	import BerryCluster from '$lib/components/svg/BerryCluster.svelte';
	import { Droplets, Heart as HeartIcon } from 'lucide-svelte';
	import confetti from 'canvas-confetti';
	import { fade, scale } from 'svelte/transition';

	let phase = $state(0);
	let activeMemory = $state<any>(null);

	const maxPhase = 5;

	// Adjusted coordinates to cluster perfectly on the large circular canopy
	// The first 4 are set up in a 2x2 grid for your test
	const memories = [
		{ id: 1, phase: 1, src: '/images/asset-1.jpg', caption: 'Where it all began.', x: '38%', y: '28%' },
		{ id: 2, phase: 1, src: '/images/asset-2.jpg', caption: 'The spark.', x: '62%', y: '28%' },
		
		{ id: 3, phase: 2, src: '/images/asset-3.jpg', caption: 'Finding home in you.', x: '38%', y: '45%' },
		{ id: 4, phase: 2, src: '/images/asset-4.jpg', caption: 'Our first adventure.', x: '62%', y: '45%' },
		
		// Optional: If you add the rest back later, they will cluster around the edges of the circle
		{ id: 5, phase: 3, src: '/images/asset-5.jpg', caption: 'Growing stronger.', x: '20%', y: '36%' },
		{ id: 6, phase: 3, src: '/images/asset-6.jpg', caption: 'Building history.', x: '80%', y: '36%' },
		{ id: 7, phase: 4, src: '/images/asset-7.jpg', caption: 'Perfectly interwoven.', x: '28%', y: '58%' },
		{ id: 8, phase: 4, src: '/images/asset-8.jpg', caption: 'Deep laughter.', x: '72%', y: '58%' },
		{ id: 9, phase: 5, src: '/images/asset-9.jpg', caption: "Magnificent growth.", x: '50%', y: '58%' },
		{ id: 10, phase: 5, src: '/images/asset-10.jpg', caption: "Endless bloom.", x: '50%', y: '16%' }
	];

	// Slightly softened the pink for the final phase to match your image
	const phaseColors = [
		'bg-slate-900',  'bg-indigo-950', 'bg-sky-800', 
		'bg-blue-500',   'bg-pink-200',  'bg-pink-300'
	];

	let currentBg = $derived(phaseColors[phase]);
	let visibleMemories = $derived(memories.filter(m => m.phase <= phase));

	function handleWateringComplete() {
		if (phase < maxPhase) {
			phase++;
			if (phase === maxPhase) {
				triggerConfetti();
			}
		}
	}

	function triggerConfetti() {
		const duration = 3000;
		const end = Date.now() + duration;

		const frame = () => {
			confetti({ particleCount: 5, angle: 60, spread: 55, origin: { x: 0 }, colors: ['#ffc0cb', '#ff69b4', '#ff1493'] });
			confetti({ particleCount: 5, angle: 120, spread: 55, origin: { x: 1 }, colors: ['#ffc0cb', '#ff69b4', '#ff1493'] });
			if (Date.now() < end) requestAnimationFrame(frame);
		};
		frame();
	}
</script>

<svelte:head>
	<title>The Garden of Us</title>
	<link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&display=swap" rel="stylesheet">
</svelte:head>

<div class="fixed inset-0 z-[100] hidden flex-col items-center justify-center bg-slate-900 p-6 text-center text-white landscape:flex sm:landscape:hidden">
	<HeartIcon class="mb-4 animate-pulse text-pink-500" size={48} />
	<h2 class="text-2xl font-serif">Please Rotate Your Device</h2>
	<p class="mt-2 text-slate-300">This experience is designed to be held upright.</p>
</div>

<main class="relative h-[100dvh] w-full overflow-hidden transition-colors duration-1000 {currentBg}">
	
	{#if phase >= 4}
		<div class="absolute inset-0 flex items-center justify-center pointer-events-none" transition:fade={{duration: 1500}}>
			<svg viewBox="0 0 24 24" class="w-[140vw] max-w-[800px] -translate-y-10 opacity-60 drop-shadow-[0_0_30px_rgba(255,255,255,0.8)]">
				<path 
					d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z" 
					fill="none" 
					stroke="#ff8da1" 
					stroke-width="0.3" 
					class="opacity-80"
				/>
				<path 
					d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z" 
					fill="#ffffff" 
					fill-opacity="0.1" 
				/>
			</svg>
		</div>
	{/if}

	<div class="absolute bottom-0 left-0 flex h-full w-full flex-col items-center justify-end pb-32">
		
		<div class="relative flex h-[550px] w-[320px] flex-col items-center justify-end pb-8">
			
			{#each visibleMemories as memory (memory.id)}
				<button
					class="absolute z-30 flex h-14 w-14 -translate-x-1/2 -translate-y-1/2 items-center justify-center rounded-full bg-white/20 shadow-[0_0_15px_rgba(255,255,255,0.8)] backdrop-blur-sm transition-all hover:scale-110 active:scale-95"
					style="left: {memory.x}; top: {memory.y};"
					onclick={() => activeMemory = memory}
					transition:scale={{ duration: 600, delay: 300, start: 0.5 }}
				>
					<div class="h-12 w-12 overflow-hidden rounded-full border-[2.5px] border-white">
						<img src={memory.src} alt="Memory Thumbnail" class="h-full w-full object-cover" />
					</div>
				</button>
			{/each}

			<div class="relative flex flex-col items-center justify-end">
				<div class="relative z-10 flex items-end justify-center">
					<div class="absolute bottom-full mb-1 h-6 w-6 rounded-full bg-[#0a7a53] transition-all duration-1000 {phase >= 1 ? 'scale-100 opacity-100' : 'scale-0 opacity-0'}"></div>
					<div class="absolute bottom-full mb-2 h-20 w-28 rounded-full bg-[#0a7a53] transition-all duration-1000 {phase >= 2 ? 'scale-100 opacity-100' : 'scale-0 opacity-0'}"></div>
					
					<div class="absolute bottom-full mb-2 h-64 w-64 rounded-full bg-[#0a7a53] shadow-inner transition-all duration-1000 {phase >= 3 ? 'scale-100 opacity-100' : 'scale-0 opacity-0'}"></div>

					{#if phase === 5}
						<div class="absolute bottom-[230px] left-1/2 z-20 flex h-0 w-0 items-center justify-center">
							
							<div class="absolute -top-12" in:scale={{ duration: 1000, start: 0.4, delay: 0 }}>
								<FlowerBloom color="text-pink-300" size="w-28 h-28" />
							</div>

							<div class="absolute -top-8 -left-20" in:scale={{ duration: 800, start: 0.2, delay: 200 }}>
								<FlowerBloom color="text-rose-400" size="w-20 h-20" />
							</div>
							<div class="absolute -top-6 right-16" in:scale={{ duration: 800, start: 0.2, delay: 400 }}>
								<FlowerBloom color="text-pink-400" size="w-24 h-24" />
							</div>
							<div class="absolute top-2 -left-28" in:scale={{ duration: 800, start: 0.2, delay: 600 }}>
								<BerryCluster color="text-red-400" size="w-16 h-16" />
							</div>
							<div class="absolute top-6 right-24" in:scale={{ duration: 800, start: 0.2, delay: 800 }}>
								<FlowerBloom color="text-rose-300" size="w-16 h-16" />
							</div>
						</div>
					{/if}
				</div>

				<div 
					class="z-0 rounded-t-sm bg-[#6b3e1b] transition-all duration-1000"
					class:w-0={phase === 0} class:h-0={phase === 0} 
					class:w-2={phase === 1} class:h-8={phase === 1}
					class:w-4={phase === 2} class:h-20={phase === 2}
					class:w-8={phase >= 3} class:h-48={phase >= 3}
				></div>
			</div>
			
			<div class="absolute bottom-0 z-20 h-16 w-48 rounded-t-[100px] bg-[#2a2a2a] shadow-lg transition-all duration-1000"></div>
		</div>

		<div class="absolute bottom-10 flex w-full flex-col items-center gap-4">
			{#if phase < maxPhase}
				<p class="animate-pulse font-serif text-sm text-white/90" transition:fade>
					{phase === 0 ? 'Plant the seed of our story...' : 'Keep nurturing to see it grow...'}
				</p>
				<HoldButton oncomplete={handleWateringComplete} duration={500}>
					<Droplets size={20} class={phase === 0 ? 'text-white' : 'text-blue-200'} />
					<span>{phase === 0 ? 'Hold to Plant' : 'Hold to Water'}</span>
				</HoldButton>
			{:else}
				<div class="text-center text-white" transition:fade={{ delay: 500 }}>
					<h2 class="mb-2 text-4xl font-bold text-white drop-shadow-md" style="font-family: 'Dancing Script', cursive;">
						Our Love is in Full Bloom
					</h2>
					<p class="font-serif text-sm opacity-90">Happy Valentine's Day!</p>
				</div>
			{/if}
		</div>
	</div>
</main>

<MemoryModal memory={activeMemory} onclose={() => activeMemory = null} />