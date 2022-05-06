<script>
    export let states;
    export let position;
    export let currentStep;
    // export let innerHeight;
    export let innerWidth;
    import { extent, max, scaleSequential, interpolateReds, scaleLinear, arc, interpolateBuPu } from 'd3';
    import Legend from './Legend.svelte';

    // cartogram variables
    let svgPadding = 5;
    let cellPadding = 2;
    $: gridHeight = max(position, ([, [, j,]]) => j) + 1
    $: gridWidth = max(position, ([, [i]]) => i) + 1
    $: cellSize = innerWidth>640?(innerWidth*0.7)/gridWidth:(innerWidth*0.9)/gridWidth;
    $: cellInner = cellSize - cellPadding * 2;
    $: w = gridWidth * cellSize + 2 * svgPadding;
    $: h = gridHeight * cellSize + 2 * svgPadding + cellSize*1.5;

    // arc variables
    let bar = ({width: 6, padding: 0});
    let minRadius = 10;
    $: innerRadius = i => minRadius + (bar.width + bar.padding);
    $: outerRadius = i => innerRadius(i) + bar.width;
    let maxRadius = 100;
    $: maxValue = max(states, d => +d.deadlyIndex);
    $: x = scaleLinear()
        .domain([0, maxValue])
        .range([0, 2 * Math.PI])
    $: arcLine = (d, i) => arc()
        .innerRadius(innerRadius(i))
        .outerRadius(outerRadius(i))
        .startAngle(0)
        .endAngle(x(+d.deadlyIndex))()

    // $: legendData = states.filter(d=>d.stateAbbrv==="NY")[0];
    // $: stateFill = currentStep === 3 ? "purple" : currentStep === 1 ? "red": currentStep === 2 ? "green": "#eee";

    $: color = scaleSequential(interpolateBuPu).domain(extent(states, d=>d.deadlyIndex))

    // $: console.log(arcLine((states.filter(d=>d.stateAbbrv==="NY")[0])))

</script>

<div class="chartElements">
    <svg width="{w}px" height="{h}px" ><!--viewBox="0 0 {w} {h}" preserveAspectRatio="xMidYMid meet" fill={stateFill}
    -->
        <Legend {cellPadding} {cellInner} {cellSize} {gridWidth} {innerWidth} {arcLine}/>
        <g transform="translate({svgPadding}, {svgPadding + cellSize*1.5})">
            {#each states as state, i}
            <g transform="translate({state.position[0] * cellSize}, {state.position[1] * cellSize})">
                <g transform="translate({cellPadding}, {cellPadding})">
                    <!-- <rect 
                    width="{cellInner}" 
                    height="{cellInner}" 
                    opacity=0.5
                    fill=#eee
                    /> -->
                    <rect 
                    width="{cellInner}" 
                    height="{cellInner}" 
                    opacity=1 
                    fill={currentStep === 1 ? color(state.deadlyIndex): state.party==="republican"?"#dd2c35":"#0080c9"}
                    />
                    <!-- <text 
                    class="stateName"
                    font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
                    x="2" 
                    y="5" 
                    text-anchor="left" 
                    dy="0.71em" 
                    font-weight=700
                    >{state.stateAbbrv}
                    </text> -->
                    <text 
                    class="stateName"
                    font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
                    x="2" 
                    y="5" 
                    text-anchor="left" 
                    dy="0.71em" 
                    font-weight=700
                    fill="white"
                    >{state.stateAbbrv}
                    </text>
                    <text 
                    class="stateName"
                    font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
                    x={cellInner/2}
                    y={cellInner/2+3}
                    text-anchor="middle" 
                    font-weight={state.deadlyIndex > 0.5 ? 700 : state.deadlyIndex > 0.7 ? 900 : 400}
                    fill={"white"}
                    >{(state.deadlyIndex*100).toFixed()}%
                    </text>
                    <!-- fill={state.party==="republican"?"#dd2c35":"#0080c9"} -->
                    <!-- <path 
                    transform="translate({cellInner/2}, {cellInner/2})"
                    d={arcLine(state, i)}
                    fill={state.party==="republican"?"#dd2c35":"#0080c9"}
                    stroke="none" 
                    stroke-width="1" /> -->
                    <path 
                    transform="translate({cellInner/2}, {cellInner/2})"
                    d={arcLine(state, i)}
                    fill={"white"}
                    stroke="none" 
                    stroke-width="1" />
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