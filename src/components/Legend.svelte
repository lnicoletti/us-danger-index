<script>
    export let cellPadding;
    export let cellInner;
    export let cellSize;
    export let gridWidth;
    export let gridHeight;
    export let svgPadding;
    export let innerWidth;
    export let currentStep;
    export let showVar;
    export let colorScale;
    import { scaleLinear, arc } from 'd3';

    let inTextColor = "white";
    let outTextColor = "grey";
    let tileColor = "lightgrey";
    // let bar = ({width: 6, padding: 0});
    // let minRadius = 10;
    $: bar = ({width: innerWidth>700?cellInner/9:cellInner/12, padding: 0});
    // $: minRadius = cellInner*0.2;
    $: minRadius = innerWidth>700?cellInner*0.17:cellInner*0.3;
    $:  innerRadius = i => minRadius + (bar.width + bar.padding);
    $:  outerRadius = i => innerRadius(i) + bar.width;
    let maxValue = 0.97
    let x = scaleLinear()
        .domain([0, maxValue])
        .range([0, 2 * Math.PI])
    $:  arcLine = (d, i) => arc()
        .innerRadius(innerRadius(i))
        .outerRadius(outerRadius(i))
        .startAngle(0)
        .endAngle(x(+d.deadlyIndex))()

    let data = [{deadlyIndex:1, position:[3,0], label:"v. high"}, 
                {deadlyIndex:0.66, position:[2,0], label:"high"}, 
                {deadlyIndex:0.33, position:[1,0], label:"low"}, 
                {deadlyIndex:0.75, position:[0,0], label:""}]

    // console.log("legend data", data)
    $: console.log("test", colorScale(0.8))


</script>
    <g transform="translate({svgPadding}, {svgPadding})">
        {#each data as d, i}
        <g transform="translate({d.position[0] * cellSize}, {d.position[1] * cellSize})">
            <g transform="translate({cellPadding}, {cellPadding})">
                <!-- <rect width="{cellInner}" height="{cellInner}" fill="#eee" opacity=0.5 /> -->
                <rect 
                width="{cellInner}" 
                height="{cellInner}" 
                fill={showVar!==null&&i!==3?colorScale(d.deadlyIndex):tileColor} 
                />
                <text 
                class="stateName"
                font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
                x="2" 
                y="5" 
                text-anchor="left" 
                dy="0.71em" 
                font-weight=700
                fill={inTextColor}
                >{i===3?"State":""}
                </text>
                <!-- <text 
                class="stateName"
                font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
                x={-cellInner*1.45} 
                y={cellInner/2} 
                text-anchor="right" 
                font-weight=400
                >Deadly index Score
                </text> -->
                <!-- <text 
                class="stateName"
                font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
                x={cellInner/2}
                y={cellInner/2+3}
                text-anchor="middle" 
                font-weight={data.deadlyIndex > 0.5 ? 700 : data.deadlyIndex > 0.7 ? 900 : 400}
                fill={"grey"}
                >{(data.deadlyIndex*100).toFixed()}%
                </text> -->
                <!-- {#if currentStep > 0} -->
                {#if showVar!=="activeBan"}
                <text 
                class="stateName"
                font-size={innerWidth > 640 ? cellInner/7 : cellInner/4}
                x={cellInner/2}
                y={cellInner/2+3}
                text-anchor="middle" 
                font-weight=700
                fill={inTextColor}
                >{i===3?"score":d.label}
                <!-- {(data.deadlyIndex*100).toFixed()}% -->
                </text>
                {:else}
                <text 
                font-size={innerWidth > 640 ? cellInner/7 : cellInner/4}
                x={cellInner/2}
                y={cellInner/2+3}
                text-anchor="middle" 
                font-weight=700
                fill={inTextColor}
                >{d.deadlyIndex===0.33?"no threat":d.deadlyIndex===0.66?"restrict":d.deadlyIndex===1?"ban":"risk"}
                </text>
                {/if}
                <!-- <path 
                transform="translate({cellInner/2}, {cellInner/2})"
                d={arcLine(data)}
                fill={"grey"}
                stroke="none" 
                stroke-width="1" /> -->
                <path 
                transform="translate({cellInner/2}, {cellInner/2})"
                d={arcLine(d)}
                fill={inTextColor}
                stroke="none" 
                stroke-width="1" />
                <!-- {/if} -->
            </g>
        </g>
        {/each}
        <!-- <text 
            class="stateName"
            font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
            x={cellInner*2.2}
            y="-6" 
            text-anchor="left" 
            dy="0.71em" 
            font-weight=700
            fill={outTextColor}
            >Danger is
        </text> -->
    </g>