<script>
    export let cellPadding;
    export let cellInner;
    export let cellSize;
    export let gridWidth;
    export let innerWidth;
    export let arcLine;

    let data = {deadlyIndex:0.75, party:"democrat"}

    console.log("legend data", data)


</script>
    <g transform="translate({(gridWidth-1)/2* cellSize}, {0.5 * cellSize})">
        <g transform="translate({cellPadding}, {cellPadding})">
            <!-- <rect width="{cellInner}" height="{cellInner}" fill="#eee" opacity=0.5 /> -->
            <rect width="{cellInner}" height="{cellInner}" fill="grey" opacity=0.5 />
            <text 
            class="stateName"
            font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
            x="2" 
            y="5" 
            text-anchor="left" 
            dy="0.71em" 
            font-weight=700
            fill="white"
            >State
            </text>
            <text 
            class="stateName"
            font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
            x={-cellInner*1.45} 
            y={cellInner/2} 
            text-anchor="right" 
            font-weight=400
            >Deadly index Score
            </text>
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
            <text 
            class="stateName"
            font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
            x={cellInner/2}
            y={cellInner/2+3}
            text-anchor="middle" 
            font-weight={data.deadlyIndex > 0.5 ? 700 : data.deadlyIndex > 0.7 ? 900 : 400}
            fill={"white"}
            >{(data.deadlyIndex*100).toFixed()}%
            </text>
            <!-- <path 
            transform="translate({cellInner/2}, {cellInner/2})"
            d={arcLine(data)}
            fill={"grey"}
            stroke="none" 
            stroke-width="1" /> -->
            <path 
            transform="translate({cellInner/2}, {cellInner/2})"
            d={arcLine(data)}
            fill={"white"}
            stroke="none" 
            stroke-width="1" />
        </g>
    </g>