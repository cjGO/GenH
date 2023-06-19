<script>
  export let label = '';
  export let id = '';
  export let min = 0;
  export let max = 100;
  export let step = 1;
  export let value = 0;
</script>

<div>
  <label for={id}>{label}: {value}</label>
  <input id={id} type="range" {min} {max} {step} bind:value />
</div>