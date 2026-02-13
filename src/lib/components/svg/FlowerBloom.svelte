<script lang="ts">
	let { 
		color = "text-pink-300", 
		size = "w-16 h-16", 
		class: className = "" 
	} = $props();
</script>

<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" class="{color} {size} {className} drop-shadow-sm overflow-visible">
	<g class="fill-current origin-center">
		<circle cx="30" cy="35" r="22" />
		<circle cx="70" cy="35" r="22" />
		<circle cx="30" cy="65" r="22" />
		<circle cx="70" cy="65" r="22" />
		<circle cx="50" cy="20" r="22" />
		<circle cx="50" cy="80" r="22" />
	</g>
	<circle cx="50" cy="50" r="15" class="text-yellow-200 fill-current" />
</svg>