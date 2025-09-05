<script lang="ts">
	import { diffChars, diffLines, diffWordsWithSpace } from "diff";

	let valueA: string = "";
	$: lengthA = valueA.length;

	let valueB: string = "";
	$: lengthB = valueB.length;

	$: mode = "word";

	$: showResult = !!(valueA || valueB);
	// $: diffResult = diffWordsWithSpace(valueA, valueB);
	$: diffResult = ((): any => {
		switch (mode) {
			case "word":
				return diffWordsWithSpace(valueA, valueB);
			case "char":
				return diffChars(valueA, valueB);
			case "line":
				return diffLines(valueA, valueB);
			default:
				return diffWordsWithSpace(valueA, valueB);
		}
	})();
</script>

<div id="header">
	<h1><span id="tilde">~</span>disparity</h1>
</div>

<div id="diff-container">
	<div class="diff">
		<div class="diff-title">Text A</div>
		<textarea bind:value={valueA} rows="10" spellcheck="false"></textarea>
		<div class="diff-bottom-tools">
			<button
				on:click={() => (valueA = "")}
				class="diff-clear"
				tabindex="-1">Clear</button
			>
			<div class="diff-char-counter">{lengthA}</div>
		</div>
	</div>
	<div class="diff">
		<div class="diff-title">Text B</div>
		<textarea bind:value={valueB} rows="10" spellcheck="false"></textarea>
		<div class="diff-bottom-tools">
			<button
				on:click={() => (valueB = "")}
				class="diff-clear"
				tabindex="-1">Clear</button
			>
			<div class="diff-char-counter">{lengthB}</div>
		</div>
	</div>
</div>

<div id="diff-mode">
	<button on:click={() => (mode = "word")} tabindex="-1">word</button>
	<button on:click={() => (mode = "char")} tabindex="-1">char</button>
	<button on:click={() => (mode = "line")} tabindex="-1">line</button>
</div>

{#if showResult}
	<div id="diff-result">
		<div class="diff-title">Result</div>
		<div id="result" style="white-space: pre-wrap;">
			{#each diffResult as part}
				{#if part.added}
					<span class="diff-add" style="white-space: pre-wrap;">
						{part.value}
					</span>
				{:else if part.removed}
					<span class="diff-remove" style="white-space: pre-wrap;">
						{part.value}
					</span>
				{:else}
					<span>{part.value}</span>
				{/if}
			{/each}
		</div>
	</div>
{/if}

<footer>
	<div>
		made with <span class="heart">♡</span> by
		<b><a href="https://fawn.moe">fawn</a></b> — source at
		<a href="https://codeberg.org/fawn/disparity">codeberg</a>
	</div>
</footer>
