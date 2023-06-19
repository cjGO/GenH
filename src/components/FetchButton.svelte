<script>
	import { gridStore } from '../routes/store.js'; // path to your store file
	import { onMount } from 'svelte';

	// These are the props that will be passed in from the parent component
	export let generationTime;
	export let numberTimelines;
	export let inferenceCutoff;
	export let inferenceTemperature;
  export let predictArray;

	// JS spread operator to add items to array
function addItem(data) {
  predictArray = [...predictArray, data];
}

	// Loading state for the button
	let loading = false;
	const apiKey = import.meta.env.VITE_API_KEY;
	// This function will be called when the button is clicked
	async function fetchData() {
    loading = true; // Set loading state to true before starting the fetch
		const url = 'https://api.genhealth.ai/predict';
		const headers = {
			'Content-Type': 'application/json',
			'Authorization': `Token ${import.meta.env.VITE_API_KEY}`
		};
		const body = JSON.stringify({
			history: [
				{
					code: '81',
					system: 'age',
					display: '81'
				},
				{
					code: '3021F',
					system: 'CPT4',
					display:
						'Left ventricular ejection fraction (LVEF) less than 40% or documentation of moderately or severely depressed left ventricular systolic function (CAD, HF)'
				},
				{
					code: 'I50.89',
					system: 'ICD10CM',
					display: 'Other heart failure'
				},
				{
					code: 'R94.4',
					system: 'ICD10CM',
					display: 'Abnormal results of kidney function studies'
				},
				{
					code: 'E87.6',
					system: 'ICD10CM',
					display: 'Hypokalemia'
				},
				{
					code: '551546933',
					system: 'NDC',
					display: 'dapagliflozin 10mg/1 ORAL TABLET, FILM COATED'
				},
				{
					code: '00-01-month',
					system: 'timegap',
					display: '00-01-month'
				},
				{
					code: '68788788308',
					system: 'NDC',
					display: 'carvedilol 6.25 MG Oral Tablet'
				},
				{
					code: '00228239096',
					system: 'NDC',
					display: 'Hydrochlorothiazide 25 MG / Spironolactone 25 MG Oral Tablet'
				},
				{
					code: '03-06-month',
					system: 'timegap',
					display: '03-06-month'
				},
				{
					code: '006158394',
					system: 'NDC',
					display: 'bumetanide 2mg/1 ORAL TABLET'
				},
				{
					code: '03-06-month',
					system: 'timegap',
					display: '03-06-month'
				},
				{
					code: '70756081430',
					system: 'NDC',
					display: 'hydrochlorothiazide 25 MG / olmesartan medoxomil 40 MG Oral Tablet'
				},
				{
					code: 'female',
					system: 'gender',
					display: 'female'
				}
			],
			num_predictions: numberTimelines,
			generation_length: generationTime,
			inference_threshold: inferenceCutoff,
			inference_temperature: inferenceTemperature
		});

		try {
			const response = await fetch(url, {
				method: 'POST',
				headers: headers,
				body: body
			});
			if (response.ok) {
				// get the response body
				const data = await response.json();
				// Update the store with the new data
				gridStore.update((grids) => [...grids, data]);
				addItem(data)
        console.log('fetch successful!')
        console.log(predictArray)

			} else {
				console.error('HTTP-Error: ' + response.status);
			}
		} catch (error) {
			console.error(error);
		}

		loading = false; // Set loading state to false after the fetch is completed
	}
</script>

<button on:click={fetchData} disabled={loading}>
	{#if loading}
		Loading...
	{:else}
		Fetch Data
	{/if}
</button>