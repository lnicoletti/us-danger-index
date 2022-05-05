<script>
	// export let name;
	import { onMount } from "svelte";
	import { csv, json, csvParseRows, scaleLinear, extent } from 'd3';
	import Cartogram from './components/Cartogram.svelte';
	import Scroll from "./components/Scrolly.svelte";

	let currentStep;
	let position = new Map;
	let femaleCrime=[];
	let abbrv=[];
	let femaleHealth=[];
	let legalStructures=[];
	let statesPop=[];
	let activeBans = [
  		 {state: 'ME', ban: ""}
		,{state: 'WI', ban: 1}
		,{state: 'VT', ban: ''}
		,{state: 'NH', ban: ''}
		,{state: 'WA', ban: ''}
		,{state: 'ID', ban: 1}
		,{state: 'MT', ban: ''}
		,{state: 'ND', ban: 1}
		,{state: 'MN', ban: ''}
		,{state: 'IL', ban: ''}
		,{state: 'MI', ban: 1}
		,{state: 'NY', ban: ''}
		,{state: 'MA', ban: ''}
		,{state: 'OR', ban: ''}
		,{state: 'NV', ban: ''}
		,{state: 'WY', ban: 1}
		,{state: 'SD', ban: 1}
		,{state: 'IA', ban: 1}
		,{state: 'IN', ban: ''}
		,{state: 'OH', ban: 1}
		,{state: 'PA', ban: ''}
		,{state: 'NJ', ban: ''}
		,{state: 'CT', ban: ''}
		,{state: 'RI', ban: ''}
		,{state: 'CA', ban: ''}
		,{state: 'UT', ban: 1}
		,{state: 'CO', ban: ''}
		,{state: 'NE', ban: ''}
		,{state: 'MO', ban: 1}
		,{state: 'KY', ban: 1}
		,{state: 'WV', ban: 1}
		,{state: 'VA', ban: ''}
		,{state: 'MD', ban: ''},
		{state: 'DE', ban: ''},
		{state: 'AZ', ban: 1},
		{state: 'NM', ban: ''},
		{state: 'KS', ban: 1},
		{state: 'AR', ban: 1}
		,{state: 'TN', ban: 1}
		,{state: 'NC', ban: 1}
		,{state: 'SC', ban: 1}
		,{state: 'DC', ban: ''}
		,{state: 'OK', ban: 1}
		,{state: 'LA', ban: 1}
		,{state: 'MS', ban: 1}
		,{state: 'AL', ban: 1}
		,{state: 'GA', ban: 1}
		,{state: 'HI', ban: ''}
		,{state: 'AK', ban: ''}
		,{state: 'TX', ban: 1}
		,{state: 'FL', ban: ''}
	]
	let states = [];
	let steps = ["step 1", "step 2", "step 3", "step 4"]

	onMount(async () => {

		await Promise.all([
			csv("/data/crime.csv"),
			csv("/data/stateAbbrv.csv"),
			csv("/data/femaleHealth.csv"),
			csv("/data/legalStructures.csv"),
			json("/data/statesPop.json")
		])
		.then((datasets)=>{
			femaleCrime = datasets[0];
			abbrv = datasets[1];
			femaleHealth = datasets[2];
			legalStructures = datasets[3];
			statesPop = datasets[4];
			position = grid`
				,  ,  ,  ,  ,  ,  ,  ,  ,  ,ME
				,  ,  ,  ,  ,WI,  ,  ,  ,VT,NH
				WA,ID,MT,ND,MN,IL,MI,  ,NY,MA,
				OR,NV,WY,SD,IA,IN,OH,PA,NJ,CT,RI
				CA,UT,CO,NE,MO,KY,WV,VA,MD,DE,
				,AZ,NM,KS,AR,TN,NC,SC,DC,  ,
				,  ,  ,OK,LA,MS,AL,GA,  ,  ,
				HI,AK,  ,TX,  ,  ,  ,  ,FL,  ,  
			`;
		});

	});


	$: states = Array.from(position).map((state)=>{
		return {
			stateAbbrv: state[0],
			stateName: state[1][2],
			// row v wade ban
			activeBan: activeBans.filter(d=>d.state===state[0])[0].ban===""?0:1,
			// legal structures (protective crime laws)
			protSHW:+legalStructures.filter(d=>d.State===state[1][2])[0]["Protect all workers from sexual harassment in the workplace, regardless of company size?"],
			firearmSurrender:+legalStructures.filter(d=>d.State===state[1][2])[0]["Require the relinquishment of firearms from abusers subject to DV protective orders?"],
			unempBenefits:+legalStructures.filter(d=>d.State===state[1][2])[0]["Guarantee unemployment benefits to victimes of domestic violence, sexual assault, and stalking?"],
			parLeave:+legalStructures.filter(d=>d.State===state[1][2])[0]["Mandate paid parental leave?"],
			minWage:+legalStructures.filter(d=>d.State===state[1][2])[0]["Set the minimum wage above the low-income threshold of $12 an hour?"],
			noCounselAbortion:+legalStructures.filter(d=>d.State===state[1][2])[0]["Allow women to have an abortion without state-mandated in-person counseling?"],
			ERA:+legalStructures.filter(d=>d.State===state[1][2])[0]["Ratify the Equal Rights Amendment?"],
			// female repr health services
			titlexNum: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[1][1],
			aProvidersNum: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[2][1],
			aProvidersPctChange: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[3][1],
			aProvidersNumChange: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[4][1],
			famPlanCentersNum: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[5][1],
			countiesWoAProvidersPct: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[6][1],
			titleXneedMetPct: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[7][1],
			pubFundCentersNeedMetPct: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[8][1],
			totalPubDollarsAbortion: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[9][1],
			totalPubDollarsFamPlanCenter: +Object.entries(femaleHealth.filter(d=>d["U.S. State"]===state[1][2])[0])[10][1],
			// female crime
			femaleCrimes: +femaleCrime.filter(d=>d.Code===state[0])[0]["2020ViolentCrimesFemales"],
			// female population
			femalePop: +statesPop.filter(d=>d.state===state[1][2])[0].femalePop,
			// total population
			totPop: +statesPop.filter(d=>d.state===state[1][2])[0].totPop,
			// cartogram position
			position: [state[1][0], state[1][1]],
		}
	});

	$: titlexNormScale = getNormScale("titlexNum", states)
	$: famPlanCentersNormScale = getNormScale("famPlanCentersNum", states)
	$: aProvidersNormScale = getNormScale("aProvidersNum", states)
	$: FamPlanCenterNormScale = getNormScale("totalPubDollarsFamPlanCenter", states)
	$: AbortionDollarsNormScale = getNormScale("totalPubDollarsAbortion", states)
	$: femaleCrimesNormScale = getNormScale("femaleCrimes", states)

	$: statesIndex = states.map(d=>({...d, 
						frhSI: 
						1-((titlexNormScale((d.titlexNum/d.femalePop))+
						famPlanCentersNormScale((d.famPlanCentersNum/d.femalePop))+
						aProvidersNormScale((d.aProvidersNum/d.femalePop))+
						(1-(d.countiesWoAProvidersPct/100))+
						FamPlanCenterNormScale((d.totalPubDollarsFamPlanCenter/d.femalePop))+
						d.noCounselAbortion+
						AbortionDollarsNormScale((d.totalPubDollarsAbortion/d.femalePop))+
						d.noCounselAbortion
						)/7),
						vcSI:
						femaleCrimesNormScale((d.femaleCrimes/d.femalePop)),
						lsSI:
						1-((d.protSHW + d.firearmSurrender + d.unempBenefits + d.parLeave + d.minWage + d.ERA)/6)
					}
					)
				).map(d=>({...d, 
						deadlyIndex: (d.activeBan + d.frhSI + d.vcSI + d.lsSI)/4
							}
						)
	);

	function grid() {
		const positionById = new Map;
		csvParseRows(String.raw.apply(String, arguments).replace(/^\n|\n$/g, ""), (row, j) => {
			row.forEach((id, i) => {
			if (id = id.trim()) {
				positionById.set(id, [i, j, abbrv.filter(d=>d.Code===id)[0].State]);
			}
			});
		});
		return positionById;
	}

	function getNormScale(variable, data) {
		let normScale = 
			scaleLinear()
			.domain(extent(data, d=>d[variable]/d.femalePop))

		return normScale
	}

	$: console.log(statesIndex)


	// $: console.log(statesIndex)

	$: outerWidth = 0
    $: innerWidth = 0
    $: outerHeight = 0
    $: innerHeight = 0
	// $: mainWidth = innerWidth>640?innerWidth*0.7:innerWidth*0.9
	// $: console.log(mainWidth)

</script>
<svelte:window bind:innerWidth bind:outerWidth bind:innerHeight bind:outerHeight />
<main>
	<h1>The <i><strong>Deadliest</strong></i> places to own a vagina in the United States</h1>
	<p>This is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph this is the intro paragraph </p>
	<div class="chart">
		<Cartogram states = {statesIndex} {position} {innerWidth} {innerHeight} {currentStep}/>
	</div>
	<Scroll bind:value={currentStep}>
		{#each steps as text, i}
		  <div class="step" class:active={currentStep === i}>
			<div class="step-content">
			  {@html text}
			</div>
		  </div>
		{/each}
	</Scroll>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 70vw;
		margin: 0 auto;
	}

	h1 {
		/* color: #ff3e00; */
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
		max-width: 70vw;
		margin:auto;
		margin-bottom:100px
		/* font-family: Georgia, 'Times New Roman', Times, serif; */
	}

	p {
		text-align: justify;
		max-width: 70vw;
		margin:auto
	}

	@media (max-width: 640px) {
		main, h1, p {
			max-width: 90vw;
		}
	}

	.chart {
    /* background: whitesmoke; */
    /* width: 400px;
    height: 400px; */
    /* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2); */
    position: sticky;
    top: 2vh;
    margin: auto;
    height:100%;
    /* display:table; */
    /* position: relative; */
    /* height: 100vh */
    /* bottom: 50%; */
  }

  /* .titleElements {
    display:table-cell;
    vertical-align:middle
  } */

  .chartElements {
    z-index: -1;
    position: relative;
  }

  /* Scrollytelling CSS */
  .step {
    height: 150vh;
    display: flex;
    place-items: center;
    justify-content: center;
    max-width: 450px;
    margin-left:auto;
    margin-right:auto;
    /* z-index: 1000; */
  }

  .step-content {
    /* background: #eeeeee;
    color: #ccc; */
    background: rgb(255, 255, 255, 0.85);
    color: black;
    /* border-radius: 5px; */
    border: 2px solid black;
    padding: 0.5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    /* transition: background 500ms ease; */
    /* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2); */
    z-index: 10;
    font-family: 'Roboto', sans-serif;
    font-weight: 400;
  }

  .step.active .step-content {
    background: rgb(255, 255, 255, 0.85);
    color: black;
  }
</style>