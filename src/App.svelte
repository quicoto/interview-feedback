<script>
	import { onMount } from "svelte";

	let items = [];
	let candidateName = '';
	const output = [];

	onMount(async function () {
		const response = await fetch('data.json');
		const data = await response.json();
		items = data.evaluationItems;
	});
</script>

<header>
	<div class="container">
		<h1 class="site-title"><img src="../assets/logo.svg" alt="Logo">	 Interview Feedback Generator</h1>
	</div>
</header>
<main class="container">
	<form>
		<h2>Basic Information</h2>
		<label for="candidate-name">Candidate Name</label>
		<input type="text" id="candidate-name" name="candidate-name" bind:value={candidateName} required>
		{#if candidateName !== ''}
			<ul>
				{#each items as item}
					<li>
						<h4 class="item-title">{ item.name }</h4>
						{#each item.options as option}
							<div class="radio-group">
								<input
								type="radio"
								id="{item.id}-{option.id}"
								name="{item.id}"
								value="{option.name}"
								on:change={() => output[item.id] = option.description.replace('{CANDIDATE}', candidateName)}
								>
								<label for="{item.id}-{option.id}">{option.name}</label>
							</div>
						{/each}
					</li>
				{/each}
			</ul>
		{/if}
	</form>
	<h2>Output</h2>
	<div class="output">{@html output.join('<br /><br />')}</div>
</main>
<footer>
	<div class="container">
		<a href="https://github.com/quicoto/interview-feedback">Fork me</a>
	</div>
</footer>

