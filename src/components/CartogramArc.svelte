<script>
    export let states;
    export let position;
    export let currentStep;
    // export let innerHeight;
    export let innerWidth;
    import { max, mean, scaleSequential, scaleSqrt, interpolateOranges, interpolateRdYlBu, interpolateRdGy, interpolateGnBu, interpolateRdPu, scaleLinear, scaleOrdinal, arc, interpolateBuPu } from 'd3';
    import { fade, draw, fly } from 'svelte/transition';
    import Legend from './Legend.svelte';

    // init current index
    let indexDict = {activeBan: "according to the Erosion of Abortion Rights Sub-Index", 
                     frhSI: "according to the Lack of Reproductive Health Services Sub-Index", 
                     vcSI: "according to the Violent Crime Against Women Sub-Index", 
                     lsSI: "according to the Lack of Legal Protections Sub-Index", 
                     deadlyIndex: "overall"};
    // init deadly state
    let deadlyState;
    let maxState;
    let maxStates;
    // radio button init
    let buttons = 1;
    // cartogram variables
    let svgPadding = 5;
    let cellPadding = 2;
    $: gridHeight = max(position, ([, [, j,]]) => j) + 1
    $: gridWidth = max(position, ([, [i]]) => i) + 1
    $: cellSize = innerWidth>1200?(innerWidth*0.5)/gridWidth:
                  innerWidth>1100?(innerWidth*0.6)/gridWidth:
                  innerWidth>1000?(innerWidth*0.65)/gridWidth:
                  innerWidth>640?(innerWidth*0.7)/gridWidth:(innerWidth*0.9)/gridWidth;
    $: cellInner = cellSize - cellPadding * 2;
    $: w = gridWidth * cellSize + 2 * svgPadding;
    $: h = gridHeight * cellSize + 2 * svgPadding + cellSize*0.2;

    // arc variables
    let bar = ({width: 6, padding: 0});
    // $: minRadius = cellInner*0.2;
    let minRadius = 10;
    $: innerRadius = i => minRadius + (bar.width + bar.padding);
    $: outerRadius = i => innerRadius(i) + bar.width;
    let maxRadius = 100;
    $: maxValue = max(states, d => Politicalsteps.includes(currentStep)?+d.deadlyIndex:+d[showVar]);
    $: x = scaleLinear()
        .domain([0, maxValue])
        .range([0, 2 * Math.PI])
    $: arcLine = (d, i) => arc()
        .innerRadius(innerRadius(i))
        .outerRadius(outerRadius(i))
        .startAngle(0)
        .endAngle(Politicalsteps.includes(currentStep)?x(+d.deadlyIndex):x(+d[showVar]))()

    // $: if (currentStep>0) {
    $: maxState = states.filter(d => +d[showVar] === maxValue);
    $: maxStates = maxState.map(d => d.stateName);
    $: deadlyState = maxState.length===1?maxState[0].stateName:null
    // }
    $: console.log("the most deadly state is", maxState, deadlyState)

    // color scales
    $: colorRW = scaleSequential(interpolateRdYlBu).domain([max(states, d=>d.activeBan), 0])
    $: colorFR = scaleSequential(interpolateRdPu).domain([0, max(states, d=>d.frhSI)])
    $: colorLS = scaleSequential(interpolateGnBu).domain([0, max(states, d=>d.lsSI)])
    $: colorVC = scaleSequential(interpolateOranges).domain([0, max(states, d=>d.vcSI)])
    $: colorDI = scaleSequential(interpolateBuPu).domain([0, max(states, d=>d.deadlyIndex)])
    $: colorPolitical = scaleOrdinal().domain(["republican", "democrat"]).range(["#dd2c35", "#0080c9"])

    // font size scale
    $: fontScale = scaleLinear()
        .domain([0, maxValue])
        .range([cellInner/20, cellInner/6])

    $: blanksteps = [0]
    $: RWsteps = [1,2,3,4,5]
    $: FRsteps = [6,7,8]
    $: VCsteps = [9,10,11]
    $: LSsteps = [12,13,14]
    $: DIsteps = [15,16,17,19,20]
    $: Politicalsteps = [18]

    $: colorScale = RWsteps.includes(currentStep) ? colorRW : 
                    FRsteps.includes(currentStep) ? colorFR: 
                    VCsteps.includes(currentStep) ? colorVC:
                    LSsteps.includes(currentStep) ? colorLS:
                    DIsteps.includes(currentStep) ? colorDI:
                    colorPolitical;

    $: showVar =    RWsteps.includes(currentStep) ? "activeBan" : 
                    FRsteps.includes(currentStep) ? "frhSI": 
                    VCsteps.includes(currentStep) ? "vcSI":
                    LSsteps.includes(currentStep) ? "lsSI":
                    DIsteps.includes(currentStep) ? "deadlyIndex":
                    "party";

    $: currentIndex = indexDict[showVar]

    $: averageBlue = mean(states.filter(d=>d.party==="democrat"), d=>d.deadlyIndex)
    $: averageRed = mean(states.filter(d=>d.party==="republican"), d=>d.deadlyIndex)
    $: partyGap = ((averageRed-averageBlue)/averageBlue)*100

    console.log("av. blue", averageBlue)

</script>

<div class="chartElements">
    <svg width="{w}px" height="{h}px" ><!--viewBox="0 0 {w} {h}" preserveAspectRatio="xMidYMid meet" fill={stateFill}
    -->
        {#if currentStep > 0}
        <Legend {svgPadding} {cellPadding} {cellInner} {cellSize} {gridWidth} {gridHeight} {innerWidth} {currentStep}/>
        {/if}
        <g transform="translate({svgPadding}, {svgPadding})">
            {#each states as state, i}
            <g transform="translate({state.position[0] * cellSize}, {state.position[1] * cellSize})">
                <g transform="translate({cellPadding}, {cellPadding})" cursor={currentStep===20?"pointer":""}>
                    <!-- <rect 
                    width="{cellInner}" 
                    height="{cellInner}" 
                    opacity=0.5
                    fill=#eee
                    /> -->
                    <!-- svelte-ignore a11y-mouse-events-have-key-events -->
                    <rect 
                    width="{cellInner}" 
                    height="{cellInner}" 
                    opacity=1 
                    fill={currentStep > 5 ? colorScale(state[showVar]):"#ccc"}
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
                    <!-- font-weight={!Politicalsteps.includes(currentStep)? state[showVar] > 0.5 ? 700 : state[showVar] > 0.7 ? 900 : 400 : state.deadlyIndex > 0.5 ? 700 : state.deadlyIndex > 0.7 ? 900 : 400} -->
                    {#if !blanksteps.includes(currentStep)}
                    <path 
                    transform="translate({cellInner/2}, {cellInner/2})"
                    d={arcLine(state, i)}
                    fill={"white"}
                    stroke="none" 
                    stroke-width="1" 
                    />
                    <circle 
                    transform="translate({cellInner/2}, {cellInner/2})"
                    cx="0"
                    cy="0"
                    fill={"white"}
                    stroke="none" 
                    r={minRadius + (bar.width*0.9 + bar.padding)}
                    opacity={deadlyState!==null?deadlyState.includes(state.stateName) ? 1 : 0:0}
                    />
                    <text 
                    class="stateName"
                    font-size={innerWidth > 640 ? cellInner/7 : cellInner/4}
                    x={cellInner/2}
                    y={cellInner/2+3}
                    text-anchor="middle" 
                    opacity={!Politicalsteps.includes(currentStep)? state[showVar] > 0.5 ? 1 : 0 :
                                                                    state.deadlyIndex > 0.5 ? 1 : 0}
                    font-weight={maxStates.includes(state.stateName)?900:300}
                    fill={deadlyState!==null?deadlyState.includes(state.stateName) ? colorScale(state[showVar]) : "white":"white"}
                    >{!blanksteps.includes(currentStep) ? !Politicalsteps.includes(currentStep)? (state[showVar]*100).toFixed()+"%":(state.deadlyIndex*100).toFixed()+"%":""}
                    </text>
                    {/if}
                    <!-- <path 
                    in:draw="{{duration: 10000}}"
                    transform="translate({cellInner/2}, {cellInner/2})"
                    d={arcLine(state, i)}
                    fill="none"
                    stroke="white" 
                    stroke-width="5" /> -->
                </g>
            </g>
            {/each}
        </g>
    </svg>
    <div class="maxState">
        <h3>
            {#if deadlyState!==null && !Politicalsteps.includes(currentStep) && currentStep!==0}
            The most dangerous state {currentIndex} is <b>{deadlyState}</b>
            {:else if deadlyState===null && !Politicalsteps.includes(currentStep) && currentStep!==0}
            There are <b>{maxStates.length}</b> dangerous states {currentIndex}
            {:else}
            On average, <b><span style="fill:#0080c9">red</span></b> states are <b>{partyGap.toFixed()}%</b> more dangerous than <b><span color="#dd2c35">blue</span></b> states.
            {/if}
        </h3>
    </div>
    {#if currentStep === 20}
        <h3>Color according to:</h3>
        <div style="text-align: center;margin:auto">
            <div style="display:flex;text-align: center;margin:auto" class="buttons">
                <div style="display:flex;flex-direction:column;text-align: center;margin-right:30px">
                    <input type=radio group={buttons} name="buttons" value={1} style="margin-left:40px;" on:click={
                        ()=>{
                            showVar="deadlyIndex"
                            colorScale=colorPolitical
                        }}>
                    <div style="max-width:100px">Political Party</div>
                </div>
                <div style="display:flex;flex-direction:column;text-align: center;margin-right:30px">
                    <input type=radio group={buttons} name="buttons" value={1} style="margin-left:40px;" on:click={
                        ()=>{
                            showVar="activeBan"
                            colorScale=colorRW
                        }}>
                    <div style="max-width:100px">Erosion of Abortion Rights Index</div>
                </div>
                <div style="display:flex;flex-direction:column;text-align: center;margin-right:30px">
                    <input type=radio group={buttons} name="buttons" value={2} style="margin-left:40px" on:click={
                        ()=>{
                            showVar="frhSI"
                            colorScale=colorFR
                        }}>
                    <div style="max-width:100px">Lack of Reproductive Health Services Index</div>
                </div>
                <div style="display:flex;flex-direction:column;text-align: center; margin-right:30px">
                    <input type=radio group={buttons} name="buttons" value={3} style="margin-left:40px" on:click={
                        ()=>{
                            showVar="vcSI"
                            colorScale=colorVC
                        }}>
                    <div style="max-width:100px">Violent Crime Against Women Index</div>
                </div>
                <div style="display:flex;flex-direction:column;text-align: center; margin-right:30px">
                    <input type=radio group={buttons} name="buttons" value={3} style="margin-left:40px" on:click={
                        ()=>{
                            showVar="lsSI"
                            colorScale=colorLS
                        }}>
                    <div style="max-width:100px">Lack of Legal Protections Index</div>
                </div>
                <div style="display:flex;flex-direction:column;text-align: center; margin-right:30px">
                    <input type=radio group={buttons} name="buttons" style="margin-left:40px" checked="checked" on:click={
                        ()=>{
                            showVar="deadlyIndex"
                            colorScale=colorDI
                        }}>
                    <div style="max-width:100px">Overall Danger Index</div>
                </div>
            </div>
        </div>
    {/if}
</div>

<style>

    .buttons {
        font-size:11px;
        font-weight:700px;
        margin-bottom:50px
    }

    h3 {
        font-size:16px;
        font-weight:400;
    }
    
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