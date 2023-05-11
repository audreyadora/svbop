<script lang="ts">
    import { onMount, onDestroy } from 'svelte';

    import AutomationTooltip from './AutomationTooltip.svelte';
    import AutomationBackground from './AutomationBackground.svelte';
    interface Point {
      x: number;
      y: number;
    }
    export let track: Point[] = [];
    export let pointInteractionBounds: number = 8
    export let pointColor: string ='lightgray'
    export let pointColorSelected: string = 'magenta'
    export let pointStrokeColor: string = 'black'
    export let pointStrokeColorSelected: string = 'black'
    export let pointStrokeWidth: number = 1
    export let pointStrokeWidthSelected: number = 1
    export let pointRadius: number = 3.5
    export let pointRadiusSelected: number = 4
    export let lineColor: string = 'black'
    export let lineWidth: number = 2.5
    export let cWidth: number = 100
    export let cHeight: number = 50 
    export let ySnapSubdivisions: number = 8;
    export let gridColor: string = 'lightgray'
    export let gridStrokeWidth: number = 0.5
    export let backgroundColor1: string = 'var(--primary-400)'
    export let backgroundColor2: string = 'darkgrey'
    export let pattern: 'none' | 'checkerboard' | 'vertical-stripes' | 'horizontal-stripes' = 'none'
    export let initPoints = 20
    $:track = Array(initPoints).fill(0).map((_, i) => {
        const xStart = pointRadiusSelected*2;
        const xEnd = cWidth - pointRadiusSelected*2;
        const x = (xStart + ((i / (initPoints - 1)) * (xEnd - xStart)));
        return {
            x: x,
            y: (cHeight / 2) 
        };
    })
    
    export let softSnap: boolean = true
    $: svgCenterY = cHeight / 2; // y-axis centerline of the SVG
    $:yRange = (cHeight / 2) - pointRadiusSelected*2; // range of y values allowed from the y-axis centerline
    $:minY = svgCenterY - yRange;
    $:maxY = svgCenterY + yRange;
    $:yGridCoordinates = Array(ySnapSubdivisions*2).fill(0).map((_,i) => i*(yRange/(ySnapSubdivisions*2)))
    $:xGridCoordinates = track.map(p=>p.x)
    $:xGridSpacing = xGridCoordinates.length > 1 ? xGridCoordinates[1]-xGridCoordinates[0] : 0
    export let showToolTip = false;
    export let selectedPointIndex = -1;
    let isDragging = false;
    let dragIndex = -1;
    export let tooltipPosition: Point = {x:0,y:0}
    let tooltipPositionAbsolute: Point = {x:0,y:0}
    // Tooltip position calculation
    function onkeydown (event: KeyboardEvent) {

            if (event.key == "Shift") {
            softSnap = false
            showToolTip =  true

            } 
         
      
    }
    function onkeyup (event: KeyboardEvent) {if (event.key == "Shift") {            
            softSnap = true
            showToolTip = false}}
    function handleMouseEnter(event: MouseEvent, index: number) {
      if (!isDragging) {
        selectedPointIndex = index;
      }
    }
  
    function handleMouseLeave() {
      if (!isDragging) {
        selectedPointIndex = -1;
      }
    }
  
    function handleMouseDown(event: MouseEvent, index: number) {
      selectedPointIndex = index;
      isDragging = true;
      dragIndex = index;
    }
  
    function handleMouseMove(event: MouseEvent) {
    if (isDragging && selectedPointIndex !== -1) {
      const { x, y } = getMousePosition(event);


      let closestIndex = selectedPointIndex;
      let closestDistance = Math.abs(track[selectedPointIndex].x - x) - xGridSpacing / 2;

      track.forEach((point, index) => {
        if (index !== selectedPointIndex) {
          const distance = Math.abs(point.x - x);
          if (distance < closestDistance) {
            closestIndex = index;
            closestDistance = distance;
          }
        }
      });
      if (softSnap) {

      selectedPointIndex = closestIndex;
      const ySnapIncrement = yRange / ySnapSubdivisions;
      const snappedY = (Math.round((Math.min(Math.max(y, minY), maxY) - minY) / ySnapIncrement) * ySnapIncrement + minY);
      track[selectedPointIndex].y = snappedY;
     
      } else {
        track[selectedPointIndex].y = (Math.min(Math.max(y, minY), maxY) - minY)   + minY
      }
      tooltipPosition = {x: x, y: y}
        const rect = (event.currentTarget as Element).getBoundingClientRect();
        tooltipPositionAbsolute = { x: rect.left + x, y: rect.top + y }
    }
  }


  
    function handleMouseUp() {
      isDragging = false;
      dragIndex = -1;
      selectedPointIndex = -1;
    }
  
    function getMousePosition(event: MouseEvent): Point {
      const rect = (event.currentTarget as Element).getBoundingClientRect();
      return {
        x: event.clientX - rect.left,
        y: event.clientY - rect.top,
      };
    }
  
    onMount(() => {
      // Add initial track points if none exist
      if (track.length === 0) {
        track = [
          { x: 0, y: 100 },
          { x: 200, y: 50 },
          { x: 400, y: 150 },
        ];
      }
  
      const handleDocumentMouseUp = () => {
        if (isDragging) {
          isDragging = false;
          dragIndex = -1;
          selectedPointIndex = -1;
        }
      };
  
      document.addEventListener('mouseup', handleDocumentMouseUp);
  
      onDestroy(() => {
        document.removeEventListener('mouseup', handleDocumentMouseUp);
      });
    });
  </script>
  




<div class="svg-wrapper">  
    <AutomationBackground pattern={"horizontal-stripes"} cHeight={cHeight} cWidth={cWidth} xGridCoordinates={xGridCoordinates} yGridCoordinates={yGridCoordinates} backgroundColor1={"bg-surface-200"} backgroundColor2={"bg-surface-100"}/>
    
    <svg viewBox="0 0 {cWidth} {cHeight}" on:mousemove={(event)=>handleMouseMove(event)} on:mouseup={handleMouseUp}>

      {#each track as point, i}
        {#if i < track.length - 1}
          <line 
                x1={point.x} 
                y1={point.y} 
                x2={track[i+1].x} 
                y2={track[i+1].y} 
                stroke={lineColor} 
                stroke-width={lineWidth} 
            />
        {/if}
      {/each}
      {#each track as point, i}

      <rect 
          x={i > 0 ? (point.x + track[i-1].x) / 2 : 0} 
          y={0} 
          width={i < track.length - 1 ? (track[i+1].x - point.x) : cWidth - point.x} 
          height={cHeight} 
          fill="none"
          pointer-events="visible"
          on:mousedown={(event) => handleMouseDown(event, i)}
          on:mouseenter={(event) => handleMouseEnter(event,i)} 
          on:mouseleave={() => handleMouseLeave()}
          on:mouseup ={() => handleMouseUp()}

  />
        <circle 
                cx={point.x} 
                cy={point.y} 
                r={i === selectedPointIndex ? pointRadiusSelected : pointRadius} 
                fill={i === selectedPointIndex ? pointColorSelected : pointColor} 
                stroke={i === selectedPointIndex ? pointStrokeColorSelected : pointStrokeColor} 
                stroke-width={i === selectedPointIndex ? pointStrokeWidthSelected : pointStrokeWidth} 
          on:mousedown={(event) => handleMouseDown(event, i)}
          on:mouseenter={(event) => handleMouseEnter(event,i)} 
          on:mouseleave={() => handleMouseLeave()}
          on:mouseup ={() => handleMouseUp()}
        />
      {/each}
    </svg>

</div>

<svelte:window on:keydown|preventDefault={onkeydown} on:keyup|preventDefault={(event) => onkeyup(event)}  />
    <AutomationTooltip tooltipPosition={tooltipPositionAbsolute} showToolTip={showToolTip} selectedPointIndex={selectedPointIndex} track={track} />
<style>

    .svg-wrapper {
        position:relative;
        min-width: 100%;
        min-height: 100%;
 
    }

    svg {
        position:absolute; 
    }
    svg rect .a{
        background-color: blue;
    }
    svg rect .b{
        background-color: red;
    }
</style>

