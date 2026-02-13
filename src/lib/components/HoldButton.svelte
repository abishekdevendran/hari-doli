<script lang="ts">
	import type { Snippet } from 'svelte';

	let { 
		oncomplete, 
		duration = 1500, 
		children 
	}: { 
		oncomplete: () => void; 
		duration?: number; 
		children: Snippet 
	} = $props();

	let progress = $state(0);
	let frame: number;
	let startTime: number;
	let holding = $state(false);

	function start(e: PointerEvent) {
		e.preventDefault(); // Prevent text selection
		holding = true;
		startTime = Date.now() - (progress * duration);
		
		const loop = () => {
			if (!holding) return;
			let elapsed = Date.now() - startTime;
			progress = Math.min(elapsed / duration, 1);
			
			if (progress >= 1) {
				holding = false;
				progress = 0;
				oncomplete();
			} else {
				frame = requestAnimationFrame(loop);
			}
		};
		frame = requestAnimationFrame(loop);
	}

	function stop() {
		holding = false;
		cancelAnimationFrame(frame);
		progress = 0;
	}
</script>

<button
	onpointerdown={start}
	onpointerup={stop}
	onpointerleave={stop}
	onpointercancel={stop}
	class="relative overflow-hidden rounded-full bg-white/20 px-8 py-4 font-semibold text-white shadow-lg backdrop-blur-md transition-transform active:scale-95 touch-none select-none border border-white/30"
>
	<div 
		class="absolute bottom-0 left-0 h-full bg-pink-400/50 transition-all duration-75 ease-linear"
		style="width: {progress * 100}%"
	></div>
	
	<div class="relative z-10 flex items-center gap-2">
		{@render children()}
	</div>
</button>