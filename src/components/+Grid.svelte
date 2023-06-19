<script>
	import { onMount } from 'svelte';

	let windowWidth = 1000; // default value
	let maxSquareWidth;

	onMount(() => {
		windowWidth = window.innerWidth;
		// Calculate maxSquareWidth once windowWidth is known
		maxSquareWidth = windowWidth / longestArray.length;
	});

	export let squares = [];
	export let hoverText = '';
	export let width = 30;
	export let height = 10;
	export let systemColorMap;

	let history = squares.history;
	let predictions = squares.predictions;
  let hoverDisplay = '';

	// predictions may have different number of events. track longest one to keep formatting
	let longestArray = predictions.reduce((a, b) => (a.length > b.length ? a : b), []);

	// Ensure width does not exceed maxSquareWidth
	$: if (width > maxSquareWidth) width = maxSquareWidth;
</script>


<div style="width: {windowWidth}px; overflow-x: auto;">
  <svg style="width: {windowWidth}px; height: {predictions.length * width}px;">
    {#each predictions as row, rowIndex}
      {#each row as square, colIndex (`${rowIndex}-${colIndex}`)}
        <rect
          x={colIndex * width}
          y={rowIndex * width}
          {width}
          height={width}
          fill={systemColorMap[square.system] || '#757575'}
          on:mouseover={() => {
            hoverText = `
              Code: ${square.code}<br>
              System: ${square.system}<br>
              Display: ${square.display}
            `;
            hoverDisplay = square.display;
          }}
          on:mouseout={() => {
            hoverText = '';
            hoverDisplay = '';
          }}
          style={hoverDisplay === square.display ? 'stroke:black;stroke-width:5' : ''}
        />
      {/each}
    {/each}
  </svg>
</div>
<p class="hover-text">{@html hoverText}</p>



<style>
  .hover-text {
    min-height: 3em; /* Adjust this value based on your font size and line-height */
  }
</style>