<script>
	import { onMount } from "svelte";

	let items = [];
	let candidateName = '';
	const output = [];
	let data = {};
	let grades = [];
	let currentGrade = 0;

	onMount(async function () {
		const response = await fetch('data.json');
		data = await response.json();
		items = data.associate;
		grades = data.grades;
	});

	function onGradeChange(event) {
		items = event.target.checked ? data.programmer : data.associate
	}

	function copyToClipboard() {
		const text =
`${output.filter(i => i).join('\n\n').replaceAll('{CANDIDATE}', candidateName)}\n
Front-end Software Engineer Level Technical Recommendation: ${grades.filter(i => i.id === currentGrade)[0].name }`;

		navigator.clipboard.writeText(text);
	}
</script>

<header>
	<div class="container">
		<h1 class="site-title"><img src="./assets/logo.svg" alt="Logo">	 Interview Feedback Generator</h1>
	</div>
</header>
<main class="container">
	<form>
		<h2>Basic Information</h2>
		<label for="candidate-name">Candidate Name</label>
		<input
			type="text"
			id="candidate-name"
			name="candidate-name"
			bind:value={candidateName}
			required
			placeholder="John">

		{#if candidateName !== ''}
		<div class="form-check form-switch">
			<input
				class="form-check-input"
				type="checkbox"
				role="switch"
				id="academy"
				value="true"
				on:change={onGradeChange}>
				<label class="form-check-label" for="academy">Academy interview</label>
		</div>
		{/if}

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
								on:change={() => output[item.id] = option.description}
								>
								<label for="{item.id}-{option.id}">{option.name}</label>
							</div>
						{/each}
					</li>
				{/each}
			</ul>
		{/if}

		{#if grades.length > 0 && candidateName !== ''}
		<label for="customRange3" class="form-label">Grade ({ grades.filter(i => i.id === currentGrade)[0].name })</label>
		<input type="range" class="form-range" min=0 max={ grades.length - 1 } bind:value={currentGrade} step=1 id="customRange3">
		{/if}
	</form>
	{#if candidateName !== ''}
		<h2>Output</h2>
		<div class="output">
			{@html output.filter(i => i).join('<br /><br />').replaceAll('{CANDIDATE}', candidateName)}

			{#if grades.length > 0 }
			<br /><br />
			<strong>Front-end Software Engineer Level Technical Recommendation</strong>: { grades.filter(i => i.id === currentGrade)[0].name }
			{/if}
		</div>
		<button type="button" class="btn btn-primary" on:click={copyToClipboard}>Copy to clipboard</button>
	{/if}
</main>
<footer>
	<div class="container">
		<a href="https://github.com/quicoto/interview-feedback">Fork me</a>
	</div>
</footer>

