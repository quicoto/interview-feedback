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

<main>
	<h1>Interview Feedback Generator</h1>

	<form>
		<label for="candidate-name">Candidate Name</label>
		<input type="text" id="candidate-name" name="candidate-name" bind:value={candidateName} required>
		{#if candidateName !== ''}
			<ul>
				{#each items as item}
					<li>
						<h4>{ item.name }</h4>
						{#each item.options as option}
							<div class="input-group">
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
	{@html output.join('<br /><br />')}
</main>

