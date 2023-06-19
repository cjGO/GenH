<script>
  export let colorMap;
</script>

<style>
  .legend {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      gap: 10px;
  }
  .legend-key {
      display: flex;
      align-items: center;
      gap: 5px;
  }
  .legend-color {
      width: 20px;
      height: 20px;
  }
</style>

<div class="legend">
  {#each Object.entries(colorMap) as [key, value]}
      <div class="legend-key">
          <svg class="legend-color">
              <rect width="20" height="20" fill={value} />
          </svg>
          <div>{key}</div>
      </div>
  {/each}
</div>
