<script>
	export let size = 'small';
	export let shadow = false;
	export let bgColor = 'inherit';
	export let textColor = 'inherit';

	let isLeftHovered;

	// console.log($$restProps);
</script>
<!-- on:click={() => alert(true)} -->
<button
	{...$$restProps}
	
	style:--buttonBgColor={bgColor}
	style:--buttonTextColor={textColor}
	class:size-lg={size === 'large'}
	class:size-sm={size === 'small'}
	class:has-left={$$slots.leftContent}
	class:shadow
>
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div
		class="left-content"
		on:mouseenter={() => (isLeftHovered = true)}
		on:mouseleave={() => (isLeftHovered = false)}
	>
		<slot name="leftContent" {isLeftHovered}></slot>
	</div>
	<slot>Fallback</slot>
</button>

<style lang="scss">
	button {
		display: flex;
		align-items: center;
		border: none;
		background-color: var(--buttonBgColor);
		color: var(--buttonTextColor);
		font-weight: bold;
		border-radius: 5px;
		cursor: pointer;
		.left-content {
			margin-right: 10px;
		}
		&:disabled {
			opacity: 0.6;
			cursor: not-allowed;
		}
		&:hover {
			background-image: linear-gradient(rgba(0, 0, 0, 0.4) 0 0);
		}
		&:active {
			background-image: linear-gradient(rgba(255, 255, 255, 0.1) 0 0);
		}
		&.size-sm {
			padding: 15px 20px;
		}
		&.size-lg {
			padding: 150px 200px;
		}
		&.shadow {
			box-shadow: 0 0 10px rgba(1, 1, 1, 0.3);
		}
	}
</style>
