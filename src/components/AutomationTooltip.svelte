<!-- AutomationTrackTooltip.svelte -->
<script lang="ts">
    interface Point {
        x:number;
        y:number;
    }
    export let tooltipPosition: Point = {x:0,y:0};
    export let showToolTip:boolean = false;
    export let selectedPointIndex: number = 0;
    export let track: Point[] = [{x:0,y:0}]
    
</script>
<!-- AutomationTrackTooltip.svelte -->

<div
  class="tooltip"
  style="
    left: {tooltipPosition.x*0.95}px;
    top: {tooltipPosition.y*1.1}px;
    visibility: {showToolTip && selectedPointIndex !== -1 ? 'visible' : 'hidden'};
  "
>
  <div class="tooltip-arrow"></div>
  <div class="tooltip-content">
    {selectedPointIndex !== -1 && `${track[selectedPointIndex].y.toFixed(2)}`}
  </div>
</div>

<style>
  .tooltip {
    position: fixed;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 0.35rem;
    background-color: #333;
    color: #fff;
    border-radius: 0.23rem;
    font-size: 0.7rem;
    pointer-events: none;


  }
  
  .tooltip-arrow {
    position: absolute;
    width: 0;
    height: 0;
    border-width: 0.35rem;
    border-style: solid;
    border-color: #333 transparent transparent transparent;
    top: -0.7rem;
    left: calc(50% - 0.35rem);
  }
  
  .tooltip-content {
    margin-top: 0.35rem;
  }
</style>
