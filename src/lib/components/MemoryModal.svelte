<script lang="ts">
	import { fade, scale } from 'svelte/transition';
	import { X } from 'lucide-svelte';

	let { memory, onclose } = $props();
</script>

{#if memory}
	<div 
		class="fixed inset-0 z-50 flex items-center justify-center bg-black/60 p-6 backdrop-blur-md"
		transition:fade={{ duration: 200 }}
	>
		<div class="absolute inset-0" onclick={onclose}></div>
		
		<div 
			class="relative w-full max-w-sm overflow-hidden rounded-3xl bg-white shadow-2xl"
			transition:scale={{ duration: 300, start: 0.95 }}
		>
			<button 
				onclick={onclose}
				class="absolute right-3 top-3 z-10 rounded-full bg-black/40 p-1.5 text-white backdrop-blur-md transition-colors hover:bg-black/60"
			>
				<X size={20} />
			</button>

			<div class="aspect-square w-full bg-gray-100">
				<img 
					src={memory.src} 
					alt="Memory" 
					class="h-full w-full object-cover"
				/>
			</div>
			
			<div class="p-6 text-center">
				<p class="font-serif text-lg text-gray-800 leading-relaxed">
					{memory.caption}
				</p>
			</div>
		</div>
	</div>
{/if}