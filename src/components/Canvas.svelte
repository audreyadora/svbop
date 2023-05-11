<script lang="ts">
    import AutomationTrack from '../components/AutomationTrack.svelte'

    let clientHeight: number;
    let clientWidth: number;
    let pointRadiusSelected: number;

    export let initPoints: number = 10;


    $:points = Array(initPoints).fill(0).map((_, i) => {return {
        x: i*10, y: 50}})
  
  </script>
  <div class="grid">
	<div style="width: 100%; height:100%"><AutomationTrack track={points} /></div>
  </div>

  <style>
	:global(body) {
		padding: 0;
	}
	
	.grid {
		width: 100vw;
		height: 100vh;
		grid-template-columns: repeat(3, 1fr);
		grid-template-rows: repeat(3, 1fr);
	}
	
	.grid div {
		position: relative;
		float: left;
		padding: 4em;
		box-sizing: border-box;
        width: 100%;
        height: 100%;
	}
</style>