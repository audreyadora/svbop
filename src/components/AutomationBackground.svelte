<script lang="ts">
    interface Point {
      x: number;
      y: number;
    }
    export let backgroundColor1: string = 'var(--primary-400)'
    export let backgroundColor2: string = 'darkgrey'
    export let pattern: 'none' | 'checkerboard' | 'vertical-stripes' | 'horizontal-stripes' = 'none'
    export let yGridCoordinates: number[] = [0]
    export let xGridCoordinates: number[] = [0]
    export let cWidth: number = 100
    export let cHeight: number = 50 
    
    const getColor = (x: number, y: number): string => {

        switch(pattern) {
            case 'checkerboard':
                return (x + y) % 2 === 0 ? backgroundColor1 : backgroundColor2
            case 'vertical-stripes':
                return x % 2 === 0 ? backgroundColor1 : backgroundColor2
            case 'horizontal-stripes':
                return y % 2 === 0 ? backgroundColor1 : backgroundColor2
            default:
                return backgroundColor1
        }
    }
</script>

<style>
    .grid {
        position:absolute;
        display: grid;
        min-height: 100%;
        min-width: 100%;

    }
    .cell {
  
        min-height: 100%;
        min-width: 100%;
    }
</style>

<div class="grid" style={`grid-template-columns: repeat(${xGridCoordinates.length}, 1fr);grid-template-rows: repeat(${yGridCoordinates.length}, 1fr);`}>
    {#each Array(yGridCoordinates.length).fill(0).map((_,i)=>i) as y }
        {#each Array(xGridCoordinates.length).fill(0).map((_,i)=>i) as x }
            <div class={`cell ${getColor(x, y)}`}></div>
        {/each}
    {/each}
</div>

