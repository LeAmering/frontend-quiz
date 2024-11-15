<script>
	let { icon, title, index, focusAction, correctAnswer, incorrectlySelected, disabled } = $props();
	let ABC = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
</script>

<button
	class="alert flex items-center gap-2 focus:outline focus:outline-primary
	{correctAnswer ? 'outline outline-success' : ''}
	{incorrectlySelected ? 'outline outline-error' : ''}"
	onfocus={focusAction}
	{disabled}
	style="margin: 0.5rem 0;"
>
	{#if icon}
		<img src={icon} alt="{title}-Icon" class="w-6 h-6" />
	{:else}
		<span class="text-lg font-semibold">{ABC[index]}</span>
	{/if}

	<span>{title}</span>

	{#if correctAnswer}
		<iconify-icon icon="mdi:check-circle" class="text-success w-5 h-5 ml-2"></iconify-icon>
	{:else if incorrectlySelected}
		<iconify-icon icon="mdi:close-circle" class="text-error w-5 h-5 ml-2"></iconify-icon>
	{/if}
</button>
