<script>
	import ListItem from '$lib/fonts/components/ListItem.svelte';
	export let data;
</script>

<br />
<h1 style="font-size: 25px;">
	Welcome to the <strong class="text-primary" style="font-size: 25px;">Fronted Quiz</strong>
</h1>
<p style="font-size: 20px;">Pick a subject to get started</p>

<br />

<div class="flex flex-col gap-4">
	{#each data.quizzes as quiz, index (quiz.title)}
		<a href="/{quiz.title.toLowerCase()}"><ListItem title={quiz.title} {index} /></a>
	{/each}
</div>
