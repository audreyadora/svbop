<script lang="ts">
    import AutomationTrack from '../components/AutomationTrack.svelte'
  import AutomationTooltip from '../components/AutomationTooltip.svelte';
    let cHeight: number;
    let cWidth: number;
    let pointRadiusSelected: number;
    interface Point {
      x: number;
      y: number;
    }
    let track: Point[] 
    let tooltipPosition:Point
    let showToolTip: boolean;
    let selectedPointIndex: number

    export let initPoints: number = 20;

 
</script>

  <div class="grid bg-surface-100">
    
    <div class="grid-div bg-surface-300" bind:clientHeight={cHeight} bind:clientWidth={cWidth}>
	<AutomationTrack  ySnapSubdivisions={20} initPoints={40} bind:selectedPointIndex bind:track={track} cHeight={cHeight} bind:pointRadiusSelected={pointRadiusSelected} cWidth={cWidth} />
</div>

  </div>

  <style>
	:global(body) {
		padding: 0;
	}
	
	.grid {
        display:grid;
		width: 100vw;
		height: 100vh;
        position:relative;
        padding-top: 10vh;
        padding-left: 10vh;

	}
	
	.grid-div {
		min-width: 70vw;
		min-height: 50vh;
        max-width: 70vw;
        max-height: 50vh;
        border: 2px solid rgb(78, 78, 78);
        border-radius: 12px;
        overflow: hidden;


   
	}
</style>