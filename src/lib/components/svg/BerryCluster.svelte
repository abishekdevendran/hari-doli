<script lang="ts">
	let { 
		color = "text-rose-500", 
		size = "w-10 h-10", 
		class: className = "" 
	} = $props();
</script>

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" class="{color} {size} {className} drop-shadow-sm overflow-visible">
	<g class="fill-current">
		<circle cx="40" cy="40" r="28" />
		<circle cx="70" cy="50" r="24" />
		<circle cx="50" cy="75" r="22" />
	</g>
</svg>