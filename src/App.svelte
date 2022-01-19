<script>

	import { csvParse } from "d3";
	import { data } from "./store.js";

	let csvFile;
	const reader = new FileReader()

	$: {
		if (csvFile) {
			reader.readAsText(csvFile[0]);
		}
	}

	$: {
		if ($data.length > 0) {
			console.log(csvParse($data));
		}
	} 

	reader.onload = function (event) {
		$data = event.target.result;
	}

	reader.onprogress = function (event) {
		if (event.loaded && event.total) {
			const percent = (event.loaded / event.total) * 100;
			console.log(`Loaded: ${Math.round(percent)}%`);
		}
	};

</script>


<input type="file" bind:files={csvFile}>

{#if csvFile && csvFile[0]}
	<div class="output">{$data}</div>
{/if}