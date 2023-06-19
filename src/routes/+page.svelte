<script>
	import Slider from '../components/+Slider.svelte';
	import Grid from '../components/+Grid.svelte';
	import FetchButton from '../components/+FetchButton.svelte';
	import ColorLegend from '../components/ColorLegend.svelte';
	import { derived, get } from 'svelte/store';
	import { gridStore } from './store.js'; // path to your store file
	import { onMount } from 'svelte';

	//  Code to render the history to html

	const history = {
		predictions: [
			[
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
			]
		],
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
		]
	};

	// Store the API calls
	let predictArray = [];

	// color legend info
	const systemColorMap = {
		NDC: '#5e35b1', // Indigo
		age: '#00897b', // Teal
		gender: '#f4511e', // Deep Orange
		ICD10CM: '#7cb342', // Lime
		CPT4: '#039be5', // Light Blue
		timegap: '#c2185b', // Pink
		'RXNORM-FREETEXT': '#6d4c41', // Brown
		other: '#757575' // Grey
	};

	// default values are set here for the sliders
	let width = 20;
	let generationTime = 10;
	let numberTimelines = 5;
	let inferenceCutoff = 0.95;
	let inferenceTemperature = 0.95;

	// code to accomodate reactive window size
	let windowWidth;

	onMount(() => {
		windowWidth = window.innerWidth;

		const handleResize = () => {
			windowWidth = window.innerWidth;
		};

		window.addEventListener('resize', handleResize);

		return () => {
			window.removeEventListener('resize', handleResize);
		};
	});
</script>

<ColorLegend colorMap={systemColorMap} />

<br />
<h1>Patient History</h1>
<Grid squares={history} {systemColorMap} />

<Slider label="Width" id="width" min="1" max="100" bind:value={width} />
<Slider
	label="Generation Time"
	id="generation_time"
	min="5"
	max="100"
	bind:value={generationTime}
/>
<Slider
	label="Number timelines"
	id="number_timelines"
	min="5"
	max="100"
	bind:value={numberTimelines}
/>
<Slider
	label="Inference Temperature"
	id="inference_temperature"
	min="0"
	max="1"
	step=".01"
	bind:value={inferenceTemperature}
/>
<Slider
	label="Inference Cutoff"
	id="inference_cutoff"
	min="0.01"
	max="1"
	step="0.01"
	bind:value={inferenceCutoff}
/>

<FetchButton
	bind:predictArray
	{generationTime}
	{numberTimelines}
	{inferenceCutoff}
	{inferenceTemperature}
/>
<button on:click={() => (predictArray = [])}>Empty Array</button>

<h1>Predictions</h1>

{#if predictArray.length > 0}
	{#each predictArray as data, index (index)}
		<Grid squares={data} {systemColorMap} {width} />
	{/each}
{:else}
	<p>No data available.</p>
{/if}

<h1>todos</h1>
<ul>
	<li>Rank unique codes by frequency in list -> sync with grids</li>
	<li>
		add history/arguments above each prediction (in case history/arguments can be changed on the
		fly)
	</li>
	<li>click a square to get taken to its webpage on athena or similar database</li>
	<li>request updated api for seed</li>
</ul>

<style>
	.grid-container {
		display: grid;
		grid-template-columns: 1fr 1fr;
		gap: 10px;
	}
</style>
