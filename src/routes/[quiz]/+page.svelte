<script>
	import { page } from '$app/stores';
	import ListItem from '$lib/fonts/components/ListItem.svelte';
	import { tweened } from 'svelte/motion';
	import { cubicOut } from 'svelte/easing';

	let { data } = $props();
	let currentQuestion = $state(0);
	let selectedAnswer = $state('');
	let score = $state(0);
	let submitted = $state(false);

	const progress = tweened((1 / data.questions.length) * 100, {
		duration: 400,
		easing: cubicOut
	});

	function handleSubmit() {
		data.questions[currentQuestion].selectedAnswer = selectedAnswer;
		if (!selectedAnswer) {
			const modal = document.getElementById('my_modal_1');
			modal.showModal();
		} else {
			if (selectedAnswer === data.questions[currentQuestion].answer) {
				score++;
			}
			submitted = true;
		}
	}

	function goHome() {
		window.location.href = '/';
	}

	function nextQuestion() {
		currentQuestion++;
		submitted = false;
		selectedAnswer = '';
		progress.set(((currentQuestion + 1) / data.questions.length) * 100);
	}
</script>

{#if currentQuestion < data.questions.length}
	{#each data.questions as question, index (question.question)}
		{#if index === currentQuestion}
			<p>Question {index + 1} of {data.questions.length}</p>
			<h2>{question.question}</h2>
			<progress class="progress w-56" value={$progress} max="100"></progress>
			{#each question.options as option, index (option)}
				{#key submitted}
					<ListItem
						title={option}
						{index}
						correctAnswer={question.selectedAnswer && question.answer === option}
						incorrectlySelected={question.selectedAnswer === option && question.answer !== option}
						focusAction={() => {
							selectedAnswer = option;
						}}
						disabled={question.selectedAnswer}
					></ListItem>
				{/key}
			{/each}
		{/if}
	{/each}
	{#if !submitted}
		<button class="btn btn-primary" onclick={handleSubmit}>Submit</button>
	{:else}
		<button class="btn btn-primary" onclick={nextQuestion}>Next Question</button>
	{/if}
{:else}
	<h2>Game Over!</h2>
	<p>Your scored {score} out of {data.questions.length} points!</p>
	<div style="margin-right: 7rem 0">
		<button class="btn btn-primary" onclick={goHome}>Home</button>

		<button class="btn btn-primary" onclick={window.location.reload()}>Play again!</button>
	</div>
{/if}

<dialog id="my_modal_1" class="modal">
	<div class="modal-box">
		<h3 class="text-lg font-bold">Warning</h3>
		<p class="py-4">No answer has been selected!</p>
		<div class="modal-action">
			<form method="dialog">
				<button class="btn">Close</button>
			</form>
		</div>
	</div>
</dialog>
