<script>
  import { randomUniform, randomInt } from "d3-random";
  import { scaleSequential } from "d3-scale";
  import { interpolatePlasma } from "d3-scale-chromatic";
  import { fly } from "svelte/transition";

  let delay = 300;

  let colourScale = scaleSequential(interpolatePlasma).domain([5, 25]);

  // initial data state: 5 circles
  let data = [10, 30, 50, 70, 90].map(
      d => ({
        x: d,
        r: randomUniform(5, 25)()
      }));

  /* update data fn: generate new random radii, but also
     slice either elements 0-4 or 1-5 (to show entering and
     exiting) */
  function updateData() {
    let startIndex = randomInt(0, 2)();
    data = [10, 30, 50, 70, 90]
      .map(d => ({ x: d, r: randomUniform(5, 25)() }))
      .slice(startIndex, startIndex + 4);
  }
</script>

<!-- we use svelte's in/out transitions for entering and existing dom elements,
     and vanilla css transitions for retained elements that change. we'll use
     d3's colour scale functions to map colour too -->
<svg>
  {#each data as d, i (d.x)}
    <circle
      in:fly="{{y: 100}}" out:fly="{{y: 100}}"
      style={"transition: all 1s ease-out"}
      cx={d.x + "%"} cy="50%" r={d.r}
      fill={colourScale(d.r)}
       />
  {/each}
</svg>

<button on:click={updateData}>
  Generate data!
</button>