<script>
    export let cellPadding;
    export let cellInner;
    export let cellSize;
    export let gridWidth;
    export let innerWidth;

</script>
    <g transform="translate({(gridWidth-1)/2* cellSize}, {0.5 * cellSize})">
        <g transform="translate({cellPadding}, {cellPadding})">
            <rect width="{cellInner}" height="{cellInner}" fill="#eee" opacity=0.5 />
            <text 
            class="stateName"
            font-size={innerWidth > 640 ? cellInner/6 : cellInner/4}
            x="2" 
            y="5" 
            text-anchor="left" 
            dy="0.71em" 
            font-weight=700
            >State
            </text>
        </g>
    </g>