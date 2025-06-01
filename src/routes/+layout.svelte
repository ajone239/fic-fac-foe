<script lang="ts">
	let { children } = $props();
</script>

<h1>Fic Fac Foe!</h1>

<nav>
	<a href="/">home</a>
	<a href="/about">about</a>
	<a href="/how_to_play">how to play</a>
</nav>

{@render children()}
