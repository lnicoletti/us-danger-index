<script>
    export let annotationData;
    export let innerWidth;
    export let cellSize;
    export let cellInner;

    $: lineHeight = innerWidth>640?10:8;
    $: r = innerWidth>640?3:1.5;

</script>

<g id="clickAnnotation">
    {#each annotationData as annotation, i}
        {#if annotation.active === true}
            {#each annotation.annotation as data, i}
                <text
                class="stateName"
                x={annotation.textX*cellSize}
                y={annotation.textY*cellSize+lineHeight*i}
                text-anchor="left"
                font-size={innerWidth > 640 ? cellInner/5 : cellInner/4}
                fill="grey"
                >{data}
                </text>
            {/each} 
            <circle cx={annotation.cx*cellSize} cy={annotation.cy*cellSize} r={r} fill="grey"/>
            <line 
            x1={annotation.lineX1*cellSize} 
            y1={annotation.lineY1*cellSize+lineHeight*annotation.annotation.length} 
            x2={annotation.lineX2*cellSize}
            y2={annotation.lineY2*cellSize} 
            stroke="grey" />
        {/if}
    {/each}
</g>