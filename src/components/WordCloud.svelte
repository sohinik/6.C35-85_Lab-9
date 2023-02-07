<script>
	export let todos_text_all = "";
	import * as d3 from "d3";
	import WordCloud from "svelte-d3-cloud";

	let words_freq_dict = freq_dict(todos_text_all);
	$: words_freq_dict = freq_dict(todos_text_all);

	function freq_dict(todos_text_all) {
		var freq = todos_text_all.split(" ").reduce(function (p, c) {
			p[c] = (p[c] || 0) + 1;
			return p;
		}, {});
		var freq_dict = Object.keys(freq).map(function (key) {
			return { text: key, count: freq[key] };
		});
		return freq_dict;
	}

	function defineWord(word) {
		console.log(word);
		console.log(this);
	}

	let rotate = 0;
	let dynamics = [words_freq_dict, rotate];
	$: dynamics = [words_freq_dict, rotate];

	function rotateWords() {
		rotate += 10;
	}

	function resetRotation() {
		rotate = 0;
	}
</script>

<button on:click={rotateWords}>Rotate Words</button>

{#key dynamics}
	<WordCloud
		words={words_freq_dict}
		width="500"
		font="Nunito"
		on:mouseover={resetRotation}
		minRotate={rotate}
		maxRotate={0 - rotate}
		scheme="schemeSet2"
		backgroundColor="#f8f7f6"
	/>
{/key}

<style>
	button {
		font-family: "Nunito", sans-serif;
		font-weight: 300;
		line-height: 2;
	}
</style>
