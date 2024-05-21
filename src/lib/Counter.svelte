<script>
	export let initialCount;
	export let maxCount = undefined;

    console.log($$props, $$restProps);

	let count = initialCount;

	//Not going to be reactive based on count1, count2
	$: string = `Count is ${count}.`;

	function increment() {
		if (count == maxCount) return;
		count += 1;
	}
</script>

<button on:click={increment}>Clicks {count}</button>

<h1>{string}</h1>

<style>
	button {
		padding: 10px;
	}
</style>
