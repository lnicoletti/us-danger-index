<script>
    export let states;
    export let position;
    export let currentStep;
    // export let innerHeight;
    export let innerWidth;
    import { extent, max, scaleSequential, interpolateReds} from 'd3';
    import Legend from './LegendOLD.svelte';

    // $: console.log("states", max(position, ([, [i]]) => i) + 1)
    // cartogram variables
    let svgPadding = 5;
    let cellPadding = 2;
    $: gridHeight = max(position, ([, [, j,]]) => j) + 1
    $: gridWidth = max(position, ([, [i]]) => i) + 1
    $: cellSize = innerWidth>640?(innerWidth*0.7)/gridWidth:(innerWidth*0.9)/gridWidth;
    $: cellInner = cellSize - cellPadding * 2;
    $: w = gridWidth * cellSize + 2 * svgPadding;
    $: h = gridHeight * cellSize + 2 * svgPadding + cellSize*1.5;
    $: stateFill = currentStep === 3 ? "purple" : currentStep === 1 ? "red": currentStep === 2 ? "green": "#eee";

    $: color = scaleSequential(interpolateReds).domain(extent(states, d=>d.deadlyIndex))

</script>

<div class="chartElements">
    <svg width="{w}px" height="{h}px" ><!--viewBox="0 0 {w} {h}" preserveAspectRatio="xMidYMid meet" fill={stateFill}
    -->
        <Legend {cellPadding} {cellInner} {cellSize} {gridWidth} {innerWidth}/>
        <g transform="translate({svgPadding}, {svgPadding + cellSize*1.5})">
            {#each states as state, i}
            <g transform="translate({state.position[0] * cellSize}, {state.position[1] * cellSize})">
                <g transform="translate({cellPadding}, {cellPadding})">
                    <rect 
                    width="{cellInner}" 
                    height="{cellInner}" 
                    opacity=0.5 
                    fill={color(state.deadlyIndex)}
                    />
                    <text 
                    class="stateName"
                    font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
                    x="2" 
                    y="5" 
                    text-anchor="left" 
                    dy="0.71em" 
                    font-weight=700
                    >{state.stateAbbrv}
                    </text>
                </g>
            </g>
            {/each}
        </g>
    </svg>
</div>

<style>

    
    /* .stateName {
        font-size: 0.8em;
    } */

    /* div {
		max-width: 70vw;
	}

    @media (min-width: 640px) {
		div {
			max-width: none;
		}
	} */
</style>