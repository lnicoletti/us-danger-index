<script>
    export let states;
    export let position;
    export let currentStep;
    export let steps;
    // export let innerHeight;
    export let innerWidth;
    import { ascending, descending, max, mean, scaleSequential, scaleSequentialQuantile, scaleSqrt, interpolateLab, interpolateOranges, interpolateBuGn, interpolateYlOrRd, interpolateRdYlBu, interpolateRdGy, interpolateGnBu, interpolateRdPu, interpolatePuOr, scaleLinear, scaleOrdinal, arc, interpolateBuPu } from 'd3';
    // import { fade, draw, fly } from 'svelte/transition';
    import Legend from './Legend.svelte';
    import Annotation from './Annotation.svelte';

    // init current index
    let indexDict = {activeBan: "Erosion of Abortion Rights", 
                     frhSI: "Lack of Reproductive Health Services", 
                     vcSI: "Violent Crime Against Women", 
                     lsSI: "Lack of Legal Protections", 
                     deadlyIndex: "overall"};

    // data for click event
    // let clickData = [{var:"activeBan", label:"Erosion of Abortion Rights"}, 
    //                  {var:"frhSI", label: "Lack of Reproductive Health Services"}, 
    //                  {var:"vcSI", label: "Violent Crime Against Women"}, 
    //                  {var:"lsSI", label: "Lack of Legal Protections"}, 
    //                  {var:"deadlyIndex", label: "overall"}];
    // let clickData = {deadlyIndex:
    //                 [{var:"activeBan", label:"Erosion of Abortion Rights", acronym:"EAR", iOffset:0, riskType:""}, 
    //                  {var:"frhSI", label: "Lack of Reproductive Health Services", acronym:"LRH", iOffset:0, riskType:"Lack"}, 
    //                  {var:"vcSI", label: "Violent Crime Against Women", acronym:"VCW", iOffset:0, riskType:"Violent"}, 
    //                  {var:"lsSI", label: "Lack of Legal Protections", acronym:"LLP", iOffset:0, riskType:"Unprotected"}, 
    //                  {var:"deadlyIndex", label: "overall", acronym:"", iOffset:0, riskType:"Danger"}],
    //                  activeBan:
    //                  [{var:"activeBan", label:"Stance on Abortion Post Row vs. Wade", acronym:"Abortion Stance", iOffset:0}],
    //                  frhSI:
    //                  [{var:"titleXNorm", label:"Below Average # of Title X Centers per 100,000", acronym:"TTX", iOffset:2},
    //                   {var:"famPlanCentersNorm", label:"Below Average # of Family Planning Centers per 100,000", acronym:"FPC", iOffset:2},
    //                   {var:"aProvidersNorm", label:"Below Average # of Abortion Providers per 100,000", acronym:"AP", iOffset:2},
    //                   {var:"countiesWoAProvidersNorm", label:"Below Average % of Counties with a Known Abortion Provider", acronym:"CAB", iOffset:2},
    //                   {var:"famPlanCenterSpendNorm", label:"Below Average Per Capita Public Expenditure For Family Planning", acronym:"FP$", iOffset:2},
    //                   {var:"CounselBeforeAbortion", label:"Women are not Allowed to get an Abortion Without Attending State-Mandated Counseling", acronym:"MCA", iOffset:2}],
    //                  vcSI:
    //                  [{var:"femaleCrimesNorm", label:"# of Violent Crimes per 100,000", acronym:"Crime 100k", iOffset:0}],
    //                  lsSI:
    //                  [{var:"protSHWNorm", label:"No Protection Against Sexual Harassment at work?", acronym:"SHW", iOffset:2},
    //                   {var:"firearmSurrenderNorm", label:"No Firearm Surrender?", acronym:"FS", iOffset:2},
    //                   {var:"unempBenefitsNorm", label:"No Unemployment Benefits?", acronym:"UB", iOffset:2},
    //                   {var:"parLeaveNorm", label:"No Parental Leave?", acronym:"PL", iOffset:2},
    //                   {var:"minWageNorm", label:"No Minimum Wage?", acronym:"MW", iOffset:2},
    //                   {var:"ERANorm", label:"No Employment Rights Act?", acronym:"ERA", iOffset:2}],
    // };

    let clickData = {deadlyIndex:
                    [{var:"activeBan", label:"Erosion of Abortion Rights", acronym:1, iOffset:0, riskType:""}, 
                     {var:"frhSI", label: "Lack of Reproductive Health Services", acronym:2, iOffset:0, riskType:"Lack"}, 
                     {var:"vcSI", label: "Violent Crime Against Women", acronym:3, iOffset:0, riskType:"Violent"}, 
                     {var:"lsSI", label: "Lack of Legal Protections", acronym:4, iOffset:0, riskType:"Unprotected"}, 
                     {var:"deadlyIndex", label: "overall", acronym:"", iOffset:0, riskType:"Danger"}],
                     activeBan:
                     [{var:"activeBan", label:"Stance on Abortion Post Row vs. Wade", acronym:1, iOffset:0}],
                     frhSI:
                     [{var:"titleXNorm", label:"# of Title X Centers per 100,000", acronym:1, iOffset:2},
                      {var:"famPlanCentersNorm", label:"# of Family Planning Centers per 100,000", acronym:2, iOffset:2},
                      {var:"aProvidersNorm", label:"# of Abortion Providers per 100,000", acronym:3, iOffset:2},
                      {var:"countiesWoAProvidersNorm", label:"% of Counties with a Known Abortion Provider", acronym:4, iOffset:2},
                      {var:"famPlanCenterSpendNorm", label:"Per Capita Public Expenditure For Family Planning", acronym:5, iOffset:2},
                      {var:"noCounselAbortion", label:"Access to abortion without state-mandated in-person counseling", acronym:6, iOffset:2}],
                     vcSI:
                     [{var:"femaleCrimesNorm", label:"# of Violent Crimes per 100,000", acronym:1, iOffset:0}],
                     lsSI:
                     [{var:"protSHW", label:"Protection of all workers from sexual harassment in the workpace, regardless of company size", acronym:1, iOffset:2},
                      {var:"firearmSurrender", label:"Required relinquishment of firearms from abusers subject to domestic violcence protective orders", acronym:2, iOffset:2},
                      {var:"unempBenefits", label:"Protection of all workers from sexual harassment in the workpace, regardless of company size", acronym:3, iOffset:2},
                      {var:"parLeave", label:"Mandated parental leave", acronym:4, iOffset:2},
                      {var:"minWage", label:"Mandated minimum wage above the low-income threshold", acronym:5, iOffset:2},
                      {var:"ERA", label:"Ratified Equal Rights Amendment", acronym:6, iOffset:2}],
    };

    // data for annotations
    $: clickAnnotation = currentStep===lastStep?
      [{textX:6.2, 
        textY:1, 
        cx: 6.5, 
        cy:2,
        lineX1:7.5,
        lineX2:6.5,
        lineY1:1,
        lineY2:2,
        annotation:["Click on a state to see", "how this score was calculated"],
        active:true
        }]:currentStep===3?
      [{textX:0, 
        textY:1.4, 
        cx: 0.5, 
        cy:2,
        lineX1:0.4,
        lineX2:0.5,
        lineY1:1.3,
        lineY2:2,
        annotation:["In Washington", "abortion is", "legally protected"],
        active:true
        }]:
    //     currentStep===3?
    //   [{textX:0, 
    //     textY:1.4, 
    //     cx: 0.5, 
    //     cy:2,
    //     lineX1:0.4,
    //     lineX2:0.5,
    //     lineY1:1.3,
    //     lineY2:2,
    //     annotation:["In New Hampshire", "abortion is not", "legally protected"],
    //     active:true
    //     }]:
    //     currentStep===4?
    //   [{textX:0, 
    //     textY:1.4, 
    //     cx: 0.5, 
    //     cy:2,
    //     lineX1:0.4,
    //     lineX2:0.5,
    //     lineY1:1.3,
    //     lineY2:2,
    //     annotation:["In New Hampshire", "abortion is not", "legally protected"],
    //     active:true
    //     }]:
        [{active:false
    }]

    $: clickDataSort = clickedState!==null?clickData[showVar].map(d=>{return {
            value: clickedState[d.var],
            label: d.label,
            acronym: d.acronym,
            iOffset: d.iOffset
    }}).sort((a, b)=>ascending(a.value, b.value)).filter(d=>d.label!=="overall"):null;

    $: console.log("click data", clickDataSort)

    $: indexLabPre = ["activeBan", "frhSI", "vcSI", "lsSI"].includes(showVar) ? "according to the " : "";
    $: indexLabAft = ["activeBan", "frhSI", "vcSI", "lsSI"].includes(showVar) ? " Sub-Index" : "";
    // politics toggle
    // $: togglePolitical=false
    // init deadly state
    let deadlyState;
    let maxState;
    let maxStates;
    // radio button init
    let buttons = 1;
    // cartogram variables
    let svgPadding = 5;
    $: cellPadding = innerWidth>800?2:1;
    $: gridHeight = max(position, ([, [, j,]]) => j) + 1
    $: gridWidth = max(position, ([, [i]]) => i) + 1
    $: cellSize = innerWidth>1200?(innerWidth*0.5)/gridWidth:
                  innerWidth>1100?(innerWidth*0.6)/gridWidth:
                  innerWidth>1000?(innerWidth*0.65)/gridWidth:
                  innerWidth>640?(innerWidth*0.7)/gridWidth:(innerWidth*0.9)/gridWidth;
    $: cellInner = cellSize - cellPadding * 2;
    $: cellInnerTtip = cellInner*3+cellPadding*4;
    $: w = gridWidth * cellSize + 2 * svgPadding;
    $: h = gridHeight * cellSize + 2 * svgPadding + cellSize*0.05; //+cellSize*1.3

    // arc variables
    $: bar = ({width: innerWidth>700?cellInner/9:cellInner/12, padding: 0});
    // $: minRadius = cellInner*0.2;
    $: minRadius = (d, i) => clickedState!==null&&d.stateAbbrv===clickedState.stateAbbrv?
                            (cellInnerTtip)*0.25:cellInner*0.17;
    $: cellCenter = (d, i) => clickedState!==null&&d.stateAbbrv===clickedState.stateAbbrv?(cellInnerTtip)/2:cellInner/2
    $: innerRadius = (d, i) => minRadius(d, i) + (bar.width + bar.padding)*i;
    $: outerRadius = (d, i) => innerRadius(d, i) + bar.width;
    let maxRadius = 100;
    $: maxValue = max(states, d => Politicalsteps.includes(currentStep)?+d.deadlyIndex:+d[showVar]);
    $: x = scaleLinear()
        .domain([0, maxValue])
        .range([0, 2 * Math.PI])
    $: arcLine = (d, i) => arc()
        .innerRadius(innerRadius(d, i))
        .outerRadius(outerRadius(d, i))
        .startAngle(0)
        .endAngle(Politicalsteps.includes(currentStep)?x(+d.deadlyIndex):x(+d[showVar]))()

    // scales for tooltipChart
    $: xTtip = scaleLinear()
        .domain([0, 1.05])
        .range([0, 2 * Math.PI])
    $: barTtip = ({width: innerWidth>700?cellInner/5.5:cellInner/5, padding: 0});
    $: minRadiusTtip = (d, i) => (cellInnerTtip)*0.21;
    $: innerRadiusTtip = (d, i) => minRadiusTtip(d, i) + (barTtip.width + barTtip.padding)*i;
    $: outerRadiusTtip = (d, i) => innerRadiusTtip(d, i) + barTtip.width;
    // $: cellCenterTtip = (cellInnerTtip)/2
    $: arcLineTtip = (d, i) => arc()
        .innerRadius(innerRadiusTtip(d, i))
        .outerRadius(outerRadiusTtip(d, i))
        .startAngle(0)
        .endAngle(xTtip(+d))()
    
    // $: xLab = scaleLinear()
    //     .domain([0, maxValue])
    //     .range([cellInnerTtip, 0])
    // $: yLab = 
    // scaleLinear()
    // .domain([0, 1])
    // .range([0, cellInnerTtip])

    // $: if (currentStep>0) {
    $: maxState = states.filter(d => +d[showVar] === maxValue);
    $: maxStates = maxState.map(d => d.stateName);
    $: deadlyState = maxState.length===1?maxState[0].stateName:null
    // }
    // $: console.log("the most deadly state is", maxState, deadlyState)

    // color scales
    // $: colorRW = scaleSequential(interpolateRdYlBu).domain([max(states, d=>d.activeBan), 0])
    // $: colorRW = scaleOrdinal().domain([0, 0.33, 0.66, 1]).range(["#5e3c99","#b2abd2","#fdb863","#e66101"])
    $: colorRW = scaleOrdinal().domain([0, 0.33, 0.66, 1]).range(["lightblue","pink","red","darkred"])
    $: colorFR = scaleSequential(interpolateBuGn).domain([0.25, max(states, d=>d.frhSI)])
    // $: colorFR = scaleSequential(interpolateLab("#f7fcb9", "#238443")).domain([0.25, max(states, d=>d.frhSI)])
    // $: colorFR = scaleSequential(interpolateLab("white", "#014242")).domain([0.25, max(states, d=>d.frhSI)])
    $: colorLS = scaleSequential(interpolateLab("#edf8b1", "#2c7fb8")).domain([0.1, max(states, d=>d.lsSI)])
    // $: colorLS = scaleSequential(interpolateGnBu).domain([0, max(states, d=>d.lsSI)])
    $: colorVC = scaleSequential(interpolateYlOrRd).domain([-0.2, max(states, d=>d.vcSI)])
    $: colorDI = scaleSequential(interpolateBuPu).domain([0, max(states, d=>d.deadlyIndex)])
    $: colorPolitical = scaleOrdinal().domain(["republican", "democrat"]).range(["#dd2c35", "#0080c9"])

    // font size scale
    $: fontScale = scaleLinear()
        .domain([0, maxValue])
        .range([cellInner/20, cellInner/6])

    // toggles for final buttons
    $: toggleDI = false;
    $: toggleRW = false;
    $: toggleFR = false;
    $: toggleVC = false;
    $: toggleLS = false;
    let togglePolitical=false;

    $: if (currentStep < lastStep) {
        toggleDI = false;
        toggleRW = false;
        toggleFR = false;
        toggleVC = false;
        toggleLS = false;
        togglePolitical=false
    }

    // $: colorScale = RWsteps.includes(currentStep)&&togglePolitical===false ? colorRW: 
    //                 FRsteps.includes(currentStep)&&togglePolitical===false ? colorFR: 
    //                 VCsteps.includes(currentStep)&&togglePolitical===false ? colorVC:
    //                 LSsteps.includes(currentStep)&&togglePolitical===false ? colorLS:
    //                 DIsteps.includes(currentStep)&&togglePolitical===false ? colorDI:
    //                 colorPolitical;
    $: colorScale = togglePolitical===false?
                        RWsteps.includes(currentStep)||toggleRW===true ? colorRW: 
                        FRsteps.includes(currentStep)||toggleFR===true ? colorFR: 
                        VCsteps.includes(currentStep)||toggleVC===true ? colorVC:
                        LSsteps.includes(currentStep)||toggleLS===true ? colorLS:
                        DIsteps.includes(currentStep)||toggleDI===true ? colorDI:
                        Politicalsteps.includes(currentStep) ? colorPolitical:
                        colorPolitical:
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

    // console.log("av. blue", averageBlue)

    // annotation and step variables
    $: blanksteps = [0,1]
    $: RWsteps = [2,3,4,5,6]
    $: FRsteps = [7,8,9,10]
    $: VCsteps = [11,12,13,14]
    $: LSsteps = [15,16,17]
    $: DIsteps = [18,19,20,21,23,24]
    $: Politicalsteps = [22]
    $: lastStep = steps.length-1

    $: highlightedState = currentStep===3?["WA"]:
                          currentStep===4?["NH"]:
                          currentStep===5?["FL"]:
                          currentStep===6?["AZ"]:
                          currentStep===9?["AK", "DC"]:
                          currentStep===10?["TX"]:
                          currentStep===13?["NY"]:
                          currentStep===14?["AR"]:
                          currentStep===17?["OR"]:
                          currentStep===19?"DC":
                          currentStep===20?"AR":null

    let highlightSteps = [3,4,5,6,9,10,13,14,17,19,20]

    let clickedState = null;
    // $: console.log(highlightedState)
    // $: console.log(currentStep)

    const handleMouseOver = (d) => {
        highlightedState=d.stateAbbrv
        // console.log(highlightedState)
    }

    const handleClick = (d) => {
        clickedState=d
        // console.log(clickedState)
    }

    const handleMouseLeave = (d) => {
        highlightedState=null
    }

    // remove tooltip on click outside the map
    // $:  if (clickedState !== null) {
    //         document.body.addEventListener("click", event => {
    //         // console.log(clickedState)
    //             clickedState=null
    //         })
    //         } else {
    //             document.body.removeEventListener("click", event => {
    //         // console.log(clickedState)
    //             clickedState=null
    //         })
    //     }


    // document.addEventListener('click', function(e) {
    //     if ( !document.body.contains(e.target) ) {
    //     // Do something when user clicked outside of wrapper element
    //         clickedState=null
    //     }
    // })

    $: console.log("test step", currentStep)
    // $: console.log(togglePolitical)

    // $: console.log("test line value", arcLine({deadlyIndex:0.5}, 10))

</script>

<div class="chartElements">
    <svg width="{w}px" height="{h}px" ><!--viewBox="0 0 {w} {h}" preserveAspectRatio="xMidYMid meet" fill={stateFill}
    -->
        {#if currentStep > 1}
            <Legend {svgPadding} {cellPadding} {cellInner} {cellSize} {gridWidth} {gridHeight} {innerWidth} {currentStep} {showVar} {colorScale} {togglePolitical}/>
        {/if}
        <!-- {#if currentStep > 0} -->
        <g transform="translate({svgPadding}, {svgPadding})">
            {#each states as state, i}
            <!-- add one g element for each state and translate it to it's cartogram position -->
                <g transform="translate({
                    clickedState!==null&&currentStep===lastStep&&state.stateAbbrv===clickedState.stateAbbrv?
                        (state.position[0]+state.xOffset) * cellSize+","+(state.position[1]+state.yOffset) * cellSize:
                        state.position[0] * cellSize+","+state.position[1] * cellSize})">
                    <!-- svelte-ignore a11y-mouse-events-have-key-events -->
                    <g 
                    transform="translate({cellPadding}, {cellPadding})" 
                    cursor={currentStep===lastStep?"pointer":""}
                    on:click={()=>{currentStep===lastStep?handleClick(state):null}}
                    id="{state.stateAbbrv}"
                    class="tile"
                    >
                    <!-- on:mouseover={()=>{currentStep===lastStep?handleMouseOver(state):null}}
                    on:mouseleave={()=>{currentStep===lastStep?handleMouseLeave(state):null}} -->
                    <!-- add rectangle + state name + path to each tile -->
                    <!-- position=relative
                    style="z-index:{state.stateAbbrv!==clickedState?-1:100}" -->
                        <!-- svelte-ignore a11y-mouse-events-have-key-events -->
                        <!-- width="{state.stateAbbrv==="NE"?cellInnerTtip:cellInner}" 
                        height="{state.stateAbbrv==="NE"?cellInnerTtip:cellInner}"  -->
                        <rect 
                        width="{clickedState!==null&&state.stateAbbrv===clickedState.stateAbbrv&&currentStep===lastStep?cellInnerTtip:cellInner}" 
                        height="{clickedState!==null&&state.stateAbbrv===clickedState.stateAbbrv&&currentStep===lastStep?cellInnerTtip:cellInner}"
                        opacity={highlightedState!==null && !highlightedState.includes(state.stateAbbrv)&&currentStep!==lastStep?0.3:1}
                        stroke={highlightSteps.includes(currentStep)?highlightedState!==null && !highlightedState.includes(state.stateAbbrv)?"none":colorScale(1):"none"}
                        stroke-width={highlightedState!==null && !highlightedState.includes(state.stateAbbrv)?0:3}
                        fill={currentStep > 1 ? colorScale!==colorPolitical?colorScale(state[showVar]):colorScale(state.party):"#ccc"}
                        style={currentStep===0||currentStep===undefined?"filter: blur(4px)":""}
                        />
                        <text 
                        class="stateName"
                        font-size={innerWidth > 640 ? cellInner/6 : cellInner/5}
                        x="2" 
                        y="5" 
                        text-anchor="left" 
                        dy="0.71em" 
                        font-weight=700
                        fill="white"
                        style={currentStep===0||currentStep===undefined?"filter: blur(4px)":""}
                        >{state.stateAbbrv}
                        </text>
                        <!-- if not on the first grey step -->
                        {#if !blanksteps.includes(currentStep)&&currentStep!==undefined}
                        <!-- if the state is clicked, then show data for click tip -->
                            {#if clickedState!==null&&currentStep===lastStep&&state.stateAbbrv===clickedState.stateAbbrv}
                                {#each clickDataSort as data, i}
                                    <path 
                                    transform="translate({cellCenter(state, i)},{cellCenter(state, i)})"
                                    d={clickDataSort.length>1?arcLineTtip(data.value+0.02, i-data.iOffset):arcLineTtip(data.value+0.02, 3)}
                                    fill={"white"}
                                    stroke={colorScale!==colorPolitical?colorScale(state[showVar]):colorScale(state.party)}
                                    stroke-width={innerWidth > 640 ? 2 : 1}
                                    />
                                    <text 
                                    transform="translate({cellCenter(state, i)},{clickDataSort.length>1?cellCenter(state, i)-innerRadiusTtip(data.value, i-data.iOffset):cellCenter(state, 3)-innerRadiusTtip(data.value, 3)})"
                                    fill={"white"}
                                    x={innerWidth > 640 ? -cellInner/7.5 : -cellInner/6.5}
                                    y="-2"
                                    font-size={innerWidth > 640 ? cellInner/6 : cellInner/5}
                                    >{data.acronym}
                                    </text>
                                    <text 
                                    transform="translate({cellInnerTtip*0.98},{innerWidth<500?cellInnerTtip*0.1:cellInnerTtip*0.08})"
                                    fill={"white"}
                                    x={innerWidth > 640 ? -cellInner/7.5 : -cellInner/6.5}
                                    y="-2"
                                    font-size={innerWidth > 640 ? cellInner/5 : cellInner/4}
                                    >X
                                    </text>
                                    <!-- data.acronym.length*6.5 -->
                                {/each}
                            {:else}
                            <!-- if the state is not clicked, then show normal dimensions -->
                                <path 
                                transform="translate({cellCenter(state, i)},{cellCenter(state, i)})"
                                d={arcLine(state, 1)}
                                fill={"white"}
                                stroke="none" 
                                stroke-width="1" 
                                />
                                <!-- <circle 
                                transform="translate({cellCenter(state, i)},{cellCenter(state, i)})"
                                cx="0"
                                cy="0"
                                fill={"white"}
                                stroke="none" 
                                r={minRadius(state, i) + (bar.width*0.9 + bar.padding)}
                                opacity={deadlyState!==null?deadlyState.includes(state.stateName) ? 1 : 0:0}
                                /> -->
                            {/if}
                            <!-- if the variable shown is not abortion stance -->
                            {#if showVar!=="activeBan"}
                                <text 
                                class="stateName"
                                font-size={clickedState!==null&&state.stateAbbrv===clickedState.stateAbbrv?
                                            innerWidth > 640 ? cellInner/4 : cellInner/3:
                                                deadlyState!==null&&deadlyState.includes(state.stateName)?
                                                    innerWidth > 640 ? cellInner/6 : cellInner/5:
                                                    innerWidth > 640 ? cellInner/7 : cellInner/5}
                                x={cellCenter(state, i)}
                                y={cellCenter(state, i)+3}
                                text-anchor="middle" 
                                opacity={clickedState!==null&&state.stateAbbrv===clickedState.stateAbbrv?1:
                                            !Politicalsteps.includes(currentStep)? 
                                                state[showVar] > 0.6 ? 1 : 0 : 
                                                state.deadlyIndex > 0.6 ? 1 : 0}
                                font-weight={clickedState!==null&&state.stateAbbrv===clickedState.stateAbbrv?700:
                                                maxStates.includes(state.stateName)?700:300}
                                fill={"white"}
                                >{!blanksteps.includes(currentStep) ? !Politicalsteps.includes(currentStep)? (state[showVar]*100).toFixed()+"%":(state.deadlyIndex*100).toFixed()+"%":""}
                                </text>
                                <!-- fill={deadlyState!==null?deadlyState.includes(state.stateName) ? colorScale!==colorPolitical?colorScale(state[showVar]):colorScale(state.party):"white":"white"} -->
                            {:else}
                            <!-- if variable shown is abortion stance -->
                                <text 
                                class="stateName"
                                font-size={clickedState!==null&&state.stateAbbrv===clickedState.stateAbbrv?
                                    innerWidth > 640 ? cellInner/4 : cellInner/3:
                                    innerWidth > 640 ? cellInner/7 : cellInner/5}
                                x={cellCenter(state, i)}
                                y={cellCenter(state, i)+3}
                                text-anchor="middle" 
                                opacity={clickedState!==null&&state.stateAbbrv===clickedState.stateAbbrv?1:
                                            !Politicalsteps.includes(currentStep)? 
                                                state[showVar] > 0.2 ? 1 : 0 : 
                                                state.deadlyIndex > 0.2 ? 1 : 0}
                                font-weight={clickedState!==null&&state.stateAbbrv===clickedState.stateAbbrv?700:
                                                maxStates.includes(state.stateName)?900:300}
                                fill={"white"}
                                >{!blanksteps.includes(currentStep) ? 
                                    state[showVar]===0.33?"low risk":state[showVar]===0.66?"high risk":state[showVar]===1?"ban":"no risk":""}
                                </text>
                                <!-- fill={deadlyState!==null?deadlyState.includes(state.stateName) ? colorScale!==colorPolitical?colorScale(state[showVar]):colorScale(state.party):"white":"white"} -->
                            {/if}
                            <!-- if the click tip is activated, also add wording under the percentage -->
                            {#if clickedState!==null&&currentStep===lastStep&&state.stateAbbrv===clickedState.stateAbbrv}
                                <text
                                class="stateName"
                                x={cellCenter(state, i)}
                                y={cellCenter(state, i)+cellInner/3}
                                text-anchor="middle" 
                                font-size={innerWidth > 640 ? cellInner/5 : cellInner/4}
                                fill="white"
                                >{clickData["deadlyIndex"].filter(d=>d.var===showVar)[0].riskType}</text>
                            {/if}
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
                <!-- bring tile to the front when clicked -->
                {#if clickedState!==null&&currentStep===lastStep}
                    <use 
                    id="use" 
                    xlink:href="#{clickedState.stateAbbrv}" 
                    transform="translate({
                        (clickedState.position[0]+clickedState.xOffset) * cellSize+","+(clickedState.position[1]+clickedState.yOffset) * cellSize})"
                    on:click={()=>{clickedState=null}}
                    />
                {/if}
            {/each}
        </g>
        <!-- {/if} -->
        <!-- Click annotation for explore app page -->
        {#if clickedState===null}
            <Annotation annotationData={clickAnnotation} {cellInner} {cellSize} {innerWidth}/>
        {/if}
    </svg>
    <!-- if in the scrolly part, show summary text for the user -->
    {#if currentStep<lastStep&&currentStep!==undefined}
        <div class="maxState" style="min-height:45px">
            <h3>
                {#if deadlyState!==null && !Politicalsteps.includes(currentStep) && currentStep>1}
                The most dangerous state {indexLabPre} <div class="specialWordWrap" style="background-color:{colorScale(0.8)}">{currentIndex}</div> {indexLabAft} is <b>{deadlyState}</b>
                {:else if deadlyState===null && !Politicalsteps.includes(currentStep) && currentStep>1}
                There are <b>{maxStates.length}</b> dangerous states {indexLabPre} <div class="specialWordWrap" style="background-color:{showVar==="activeBan"?colorScale(1):colorScale(0.8)}">{currentIndex}</div> {indexLabAft}
                {:else if Politicalsteps.includes(currentStep)}
                On average, <div class="specialWordWrap" style="background-color:#dd2c35">red</div> states are <b>{partyGap.toFixed()}%</b> more dangerous than <div class="specialWordWrap" style="background-color:#0080c9">blue</div> states.
                {/if}
            </h3>
        </div>
    <!-- {:else if currentStep!==undefined}
        <div class="maxState" style="min-height:45px"></div> -->
    {:else if currentStep!==undefined}
    <!-- if outside of the scrolly part, show acronym legend -->
    <div 
    class="legend"
    style="text-align:center;margin-bottom:7px;min-height:80px"
    >
        <div 
        style="text-align:left;font-size:{innerWidth > 640 ? cellInner/7 : cellInner/4}px;color:grey;max-width:400px;display:inline-block;"
        >*<br>
            {#each clickData[showVar].filter(d=>d.label!=="overall") as data, i}
                <span
                >
                    <b>{data.acronym}</b>: {data.label}
                </span><br>
            {/each}
        </div>
    </div>
    {/if}
    <!-- if outside of the scrolly part, show explore buttons -->
    {#if currentStep === lastStep}
            <div class="buttons">
                <div class="checkBoxContainer">
                    <!-- <div> -->
                        <input 
                        class="checkbox"
                        type=checkbox group={buttons} 
                        name="buttons" 
                        bind:checked={togglePolitical}
                        style="background-color:{colorPolitical("democrat")};border:0px solid {colorPolitical("republican")}"
                        on:click={
                            ()=>{
                                // showVar="deadlyIndex"
                                // togglePolitical= !togglePolitical
                                // colorScale=colorPolitical
                                // console.log(togglePolitical)
                            }}>
                        <span class="buttonLabel">Color by Political Affiliation</span>
                        <!-- <input 
                        type=checkbox group={buttons} 
                        name="buttons" 
                        value="politics" 
                        style="background-color:{colorPolitical("democrat")};border:0px solid {colorPolitical("republican")}"
                        on:click={
                            ()=>{
                                // showVar="deadlyIndex"
                                togglePolitical= !togglePolitical
                                colorScale=colorPolitical
                                // console.log(togglePolitical)
                            }}>
                        <span class="buttonLabel">View by Political Affiliation</span> -->
                 </div>
            </div>
            <div class="buttons">
                <div class="buttonContainer">
                <!-- <div> -->
                    <input 
                    type=button 
                    group={buttons} 
                    name="buttons" 
                    value={"Abortion Rights"}
                    style="background-color:{colorRW(1)};border:0px solid {colorRW(1)}" 
                    on:click={
                        ()=>{
                            showVar="activeBan"
                            // colorScale=togglePolitical===true?colorPolitical:colorRW
                            toggleRW = true
                            toggleFR = false
                            toggleVC = false
                            toggleLS = false
                            toggleDI = false
                            // colorScale=togglePolitical===false?colorRW:colorPolitical
                            // togglePolitical=false
                        }}>
                    <!-- <span class="buttonLabel">Erosion of Abortion Rights Index</span> -->
                </div>
                <div class="buttonContainer">
                <!-- <div> -->
                    <input 
                    type=button 
                    group={buttons} 
                    name="buttons" 
                    value={"Repr. Health Services Lack"} 
                    style="background-color:{colorFR(0.7)};border:0px solid {colorFR(1)}"
                    on:click={
                        ()=>{
                            showVar="frhSI"
                            toggleRW = false
                            toggleFR = true
                            toggleVC = false
                            toggleLS = false
                            toggleDI = false
                            // colorScale=togglePolitical===true?colorPolitical:colorFR
                            // toggleFR = true
                            // togglePolitical=false
                        }}>
                    <!-- <span class="buttonLabel">Lack of Reproductive Health Services Index</span> -->
                </div>
                <div class="buttonContainer">
                <!-- <div> -->
                    <input 
                    type=button 
                    group={buttons} 
                    name="buttons" 
                    value={"Violent Crime ag. Women"} 
                    style="background-color:{colorVC(0.7)};border:0px solid {colorVC(1)}}"
                    on:click={
                        ()=>{
                            showVar="vcSI"
                            toggleRW = false
                            toggleFR = false
                            toggleVC = true
                            toggleLS = false
                            toggleDI = false
                            // colorScale=togglePolitical===true?colorPolitical:colorVC
                            // toggleVC = true
                            // togglePolitical=false
                        }}>
                    <!-- <span class="buttonLabel">Violent Crime Against Women Index</span> -->
                </div>
                <div class="buttonContainer">
                <!-- <div> -->
                    <input 
                    type=button 
                    group={buttons} 
                    name="buttons" 
                    value={"Legal Protections Lack"} 
                    style="background-color:{colorLS(0.7)};border:0px solid {colorLS(1)}"
                    on:click={
                        ()=>{
                            showVar="lsSI"
                            toggleRW = false
                            toggleFR = false
                            toggleVC = false
                            toggleLS = true
                            toggleDI = false
                            // colorScale=togglePolitical===true?colorPolitical:colorLS
                            // toggleLS = true
                            // togglePolitical=false
                        }}>
                    <!-- <span class="buttonLabel">Lack of Legal Protections Index</span> -->
                </div>
                <div class="buttonContainer">
                <!-- <div> -->
                    <input 
                    type=button 
                    group={buttons} 
                    name="buttons"  
                    checked="checked" 
                    value="Danger Index" 
                    style="background-color:{colorDI(1)};border:0px solid {colorDI(1)}"
                    on:click={
                        ()=>{
                            showVar="deadlyIndex"
                            toggleRW = false
                            toggleFR = false
                            toggleVC = false
                            toggleLS = false
                            toggleDI = true                            
                            console.log(toggleDI)
                            // colorScale=togglePolitical===true?colorPolitical:colorDI
                            // togglePolitical=false
                        }}>
                    <!-- <span class="buttonLabel">Overall Danger Index</span> -->
                </div>
            </div>
            <h5>Choose a view</h5>
    {/if}
</div>

<style>

    .chartElements {
        /* z-index: -1;
        position: relative; */
        /* margin: 0; */
        position: relative;
        top: 50%;
        -ms-transform: translateY(-50%);
        transform: translateY(-50%);
    }

    .buttonLabel {
        width:8vw;
        font-family: "Roboto Flex", sans-serif;
        font-size:11px;
        font-weight:700px;
    }

    .buttonContainer {
        display:flex;
        flex-direction:column;
        text-align: center; 
        margin-right:5px;
        margin-left:5px;
    }

    input {
        /* margin-left:40%; */
        cursor: pointer;

        /* alternate */
        font-family: "Roboto Flex", sans-serif;
        font-size:11px;
        font-weight:400;
        color:white;
        text-transform: uppercase;
        border-width:0px;
        padding:5px;
        /* transform:skew(-.312rad); */
    }

    text {
        font-family: "Roboto Flex", sans-serif;
    }

    input:hover {
        /* border-width: 2px; */
        opacity: 0.5;
        /* filter:invert(100%); */
    }

    input:focus {
        font-weight: 700;
        /* border-width: 2px; */
    }

    .checkbox {
        margin-bottom: 15px;
    }
    .checkBoxContainer {
        align-items: center;
        font-family: "Roboto Flex", sans-serif;
    }

    .buttons {
        
        /* text-align: center; */
        display:flex;
        text-align: center;
        margin:auto;
        flex-wrap: wrap;
        justify-content: center;
        /* max-width: 50vw; */
        }
    .buttonsContainer {
        /* margin-bottom:50px;
        margin-top:10px; */
        /* margin-left:auto;
        margin-right: auto; */
        /* text-align: center; */
        
        /* margin:auto;
        max-width: 70vw; */
        /* display:flex;
        text-align: center;
        margin:auto;
        flex-wrap: wrap; */
        
        /* text-align: center;
        margin:auto;
        max-width: 50vw; */
    }

    /* span {
        
    } */

    h3 {
        /* color: black; */
		font-family: 'Roboto Flex', sans-serif;
		font-weight: 300;
	}

    h5 {
        color: black;
		font-family: 'Roboto Flex', sans-serif;
		font-weight: 400;
        margin-bottom: 10px;
        margin-top: 0px
	}

    

    .specialWordWrap {
        /* background-color: #730f71; */
        padding-right: 5px;
        padding-left: 5px;
        display: inline-block;
        /* transform:skew(-.312rad); */
        color: white;
        font-weight: 500;
        border-radius: 2.5px;
    }
    
        .stateName {
            font-family: 'Roboto Flex', sans-serif;
        }

        .legend {
            font-family: 'Roboto Flex', sans-serif;
            fill:"#ccc"
        }

    /* div {
		max-width: 70vw;
	}

    @media (min-width: 640px) {
		div {
			max-width: none;
		}
	} */
    @media (max-width: 640px) {
		.buttonsContainer {
			max-width: 90vw;
		}
	}
</style>