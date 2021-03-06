<script>
	// export let name;
	import { onMount } from "svelte";
	import { csv, json, csvParseRows, scaleLinear, extent, selectAll} from 'd3';
	import CartogramArc from './components/CartogramArc.svelte';
	import Scroll from "./components/Scrolly.svelte";
	import Header from "./components/Header.svelte";
	import Footer from "./components/Footer.svelte";

	let currentStep;
	let position = new Map;
	let femaleCrime=[];
	let abbrv=[];
	let femaleHealth=[];
	let legalStructures=[];
	let statesPop=[];
	let stateParty=[];
	let activeBans = [
		{state: 'ME', ban: 0}
		,{state: 'WI', ban: 1}
		,{state: 'VT', ban: 0}
		,{state: 'NH', ban: 0.33}
		,{state: 'WA', ban: 0}
		,{state: 'ID', ban: 1}
		,{state: 'MT', ban: 0.66}
		,{state: 'ND', ban: 1}
		,{state: 'MN', ban: 0}
		,{state: 'IL', ban: 0}
		,{state: 'MI', ban: 1}
		,{state: 'NY', ban: 0}
		,{state: 'MA', ban: 0}
		,{state: 'OR', ban: 0}
		,{state: 'NV', ban: 0}
		,{state: 'WY', ban: 1}
		,{state: 'SD', ban: 1}
		,{state: 'IA', ban: 1}
		,{state: 'IN', ban: 0.66}
		,{state: 'OH', ban: 1}
		,{state: 'PA', ban: 0.33}
		,{state: 'NJ', ban: 0}
		,{state: 'CT', ban: 0}
		,{state: 'RI', ban: 0}
		,{state: 'CA', ban: 0}
		,{state: 'UT', ban: 1}
		,{state: 'CO', ban: 0}
		,{state: 'NE', ban: 0.66}
		,{state: 'MO', ban: 1}
		,{state: 'KY', ban: 1}
		,{state: 'WV', ban: 1}
		,{state: 'VA', ban: 0.33}
		,{state: 'MD', ban: 0},
		{state: 'DE', ban: 0},
		{state: 'AZ', ban: 1},
		{state: 'NM', ban: 0.33},
		{state: 'KS', ban: 0},
		{state: 'AR', ban: 1}
		,{state: 'TN', ban: 1}
		,{state: 'NC', ban: 0.66}
		,{state: 'SC', ban: 1}
		,{state: 'DC', ban: 0}
		,{state: 'OK', ban: 1}
		,{state: 'LA', ban: 1}
		,{state: 'MS', ban: 1}
		,{state: 'AL', ban: 1}
		,{state: 'GA', ban: 1}
		,{state: 'HI', ban: 0}
		,{state: 'AK', ban: 0}
		,{state: 'TX', ban: 1}
		,{state: 'FL', ban: 0.66}
	]
	let states = [];
	let loaded = false;

	// border color based on steps
	$: blanksteps = [0,1,2]
    $: RWsteps = [3,4,5,6,7]
    $: FRsteps = [8,9,10,11]
    $: VCsteps = [12,13,14,15]
    $: LSsteps = [16,17,18,19]
    $: DIsteps = [20,21,22,23,25,26]
    $: Politicalsteps = [24]

	$: stepBorderColor = RWsteps.includes(currentStep)?"pink":
						 FRsteps.includes(currentStep)?"rgb(76, 179, 131)":
						 VCsteps.includes(currentStep)?"rgb(200, 10, 34)":
						 LSsteps.includes(currentStep)?"rgb(56, 131, 184)":
						 DIsteps.includes(currentStep)?"rgb(130, 35, 136)":
						 Politicalsteps.includes(currentStep)?"#0080c9":"#ccc"
	
	let steps = 
				["<div class='questions' style='text-align:center'>Firstly, what constitutes danger?<br><br><br><br><br>Additionally, how does danger vary across states?</div>",
				"<div class='specialPar'>In creating the <span style='font-style:italic'>Overall Danger Index</span> for our tool, several sub-indices were considered:<br><div style='text-align:center'><div class='specialWordWrapPar' style='border:2px solid pink'>the erosion of abortion rights</div><br><div class='specialWordWrapPar' style='border:2px solid rgb(76, 179, 131)'>reproductive health services support</div><br><div class='specialWordWrapPar' style='border:2px solid rgb(200, 10, 34)'>violent crimes committed against women</div><br><div class='specialWordWrapPar' style='border:2px solid rgb(56, 131, 184)'>state-level legal protections</div></div></div>",
				"<div>As outlined by a cartogram approach, we consider each of these sub-indices and, ultimately, our <i>Overall Danger Index</i> at the state-level.</div>", 
				 "<div>First, in creating the <i>Erosion of Abortion Rights</i> Sub-Index, we have considered state stances on banning or greatly restricting access to abortion if Roe v. Wade is to be overturned.<br><br>In the following cartogram, each square represents a US state, and the level of completion of each circle within each square represents the risk to the right of abortion imposed by each state.  For example, squares (i.e., states) denoted by full circles have trigger laws that would ban abortion.  Squares denoted by circles that are two-thirds completed represent states that are likely to restrict access to abortion.  Squares marked by one-third completed circles indicate states that pose no imminent threat to access to abortion.  Finally, if not marked with a complete or incomplete circle, a state legally protects abortion and an overturn of Roe v. Wade does not pose a risk to residents of that state.</div>", 
				 "<div>In the following example, we can see that Washington legally protects rights to abortion, regardless of a Roe v. Wade overturning.</div>",
				 "<div>It can also be observed that while New Hampshire does not have laws that legally protect the rights to abortion, an overturning of Roe v. Wade is unlikely to threaten residents' access to abortion.</div>",
				 "<div>However, in states like Florida, if Roe v. Wade is to be overturned, state action to try and ban abortion is likely.</div>",
				 "<div>Finally, states like Arizona have trigger laws in place that will ban abortion if Roe v. Wade is overturned.</div>",
				 "<div>In a similar vein, the <i>Reproductive Health Services</i> Sub-Index was then created in order to better understand the inaccessibility of reproductive health services per state. In calculating this sub-index, the number of Title X centers, publicly funded family planning centers, and abortion providers per state were considered. Per capita public expenditure for family planning was also considered.  Additionally, state mandatory in-person counseling for access to abortion was noted. For each state, the distribution of reproductive services was also taken into account, and the percentage of state counties with a known abortion provider was considered.  Finally, state population was also noted.</div>", 
				 "<div>With the same cartogram approach, each state's <i>Reproductive Health Services</i> Sub-Index score can be observed. Here, squares denoted by full circles represent states that score 100% on the <i>Reproductive Health Services</i> Sub-Index.  This means that these states provide the most reproductive health support to residents when compared to other states. Contrastingly, squares denoted by partial circles with smaller accompanying percentages represent states that provide fewer reproductive health support to residents.</div>",
				 "<div>For example, it can be observed that both Alaska (58%) and Washington D.C. (58%) provide the most reproductive health services to residents.</div>",
				 "<div>Contrastingly, Texas (6%) lacks the most in reproductive health services for residents.</div>",
				 "<div>We then considered instances of homicide, rape, robbery, and aggravated assault committed against women per state in order to calculate the <i>Violent Crime Against Women</i> Sub-Index.  In calculating this sub-index, state-level violent crime data for the year 2020 was considered in relation to the most recent state-level female population data.</div>", 
				 "<div>Similarly to the previous cartograms and sub-indices, the following cartogram outlines each state's <i>Violent Crime Against Women</i> Sub-Index score.  Here, squares denoted by complete circles or almost-complete circles represent states that score highest on the <i>Violent Crime Against Women</i> Sub-Index.  In these states, women are subjected to higher levels of violent crime.  Squares marked by partial circles with smaller accompanying percentages represent states where women are subjected to lower levels of violent crime.</div>", 
				 "<div>Here, we can see that women are subjected to the least amount of violent crime in the state of New York (11 violent crimes/100k women).</div>", 
				 "<div>In Arkansas (688 violent crimes/100k women), however, women are subjected to the most amount of violent crime.</div>",
				 "<div>Finally, in creating the <i>Legal Protections</i> Sub-Index, we have considered whether or not a state offers critical justice-based legal protections for women's rights as outlined by the <a href='https://giwps.georgetown.edu/us-index-legal-protections/' target=__blank>Georgetown Institute for Women, Peace, and Security (GIWPS)</a>. In calculating this sub-index, we considered whether or not a state has laws for: 1) protection of all workers from sexual harassment in the workpace, regardless of company size; 2) guaranteed unempoyment benefits for victims of domestic violence, sexual assault, or stalking; 3) mandated parental leave; 4) mandated minimum wage above the low-income threshold; 5) ratified Equal Rights Amendment; and 6) required relinquishment of firearms from abusers subject to domestic violence protective orders.</div>", 
				 "<div>In this cartogram, each state's score for the <i>Legal Protections</i> Sub-Index can be observed.  States that have the most legal protections for residents are indicated by squares with complete or almost-complete circles. States that have the least legal protections for residents are marked by squares with partial circles and smaller accompanying percentages.</div>", 
				 "<div>In our last example, we can see that Oregon (83%) provides the most legal protections for residents.</div>",
				 "<div>Shockingly, residents of Idaho, Utah, Nebraska, Missouri, Kentucky, Delaware, Arkansas, North Carolina, South Carolina, Louisiana, Mississippi, Alabama, and Florida do not have any legal protections.</div>", 
				 "<div>In considering all of these sub-indices, we created our <span style='font-style:italic'>Overall Danger Index</span>.</div>", 
				 "<div>As illustrated here, Washington D.C. (20%) scores the lowest on our <span style='font-style:italic'>Overall Danger Index</span>,  rendering it the safest place for female assigned at birth (AFAB) and femme folx to reside in.</div>", 
				 "<div>On the other hand, with a calculated value of 97%, Arkansas scores the highest on our <span style='font-style:italic'>Overall Danger Index</span>, making it the most dangerous place for female assigned at birth (AFAB) and femme folx to take up residence.</div>", 
				 "<div>But what do politics have to do with it?",
				 "<div>Our analysis illustrates that red states (i.e., states whose voters predominantly choose the Republican Party) are consistently more dangerous than blue states (i.e., states whose voters predominantly choose the Democratic Party) for female assigned at birth (AFAB) and femme folx.  In fact, our <span style='font-style:italic'>Overall Danger Index</span> indicates that, on average, red states are 74% more dangerous than blue states for these people.</div>",
				 "<div>Interested in learning more? Check out our tool and explore the data for yourself.</div>",
				 "<div id='lastStep'>explore the app</div>"
	];

	onMount(async () => {

		await Promise.all([
			csv("/data/crime.csv"),
			csv("/data/stateAbbrv.csv"),
			csv("/data/femaleHealth.csv"),
			csv("/data/legalStructures.csv"),
			json("/data/statesPop.json"),
			json("/data/statesParty.json"),
		])
		.then((datasets)=>{
			femaleCrime = datasets[0];
			abbrv = datasets[1];
			femaleHealth = datasets[2];
			legalStructures = datasets[3];
			statesPop = datasets[4];
			stateParty = datasets[5];
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
		loaded=true;

	});


	$: states = Array.from(position).map((state)=>{
		return {
			stateAbbrv: state[0],
			stateName: state[1][2],
			// row v wade ban
			activeBan: activeBans.filter(d=>d.state===state[0])[0].ban,
			// legal structures (protective crime laws)
			protSHW:+legalStructures.filter(d=>d.State===state[1][2])[0]["Protect all workers from sexual harassment in the workplace, regardless of company size?"],
			firearmSurrender:+legalStructures.filter(d=>d.State===state[1][2])[0]["Require the relinquishment of firearms from abusers subject to DV protective orders?"],
			unempBenefits:+legalStructures.filter(d=>d.State===state[1][2])[0]["Guarantee unemployment benefits to victimes of domestic violence, sexual assault, and stalking?"],
			parLeave:+legalStructures.filter(d=>d.State===state[1][2])[0]["Mandate paid parental leave?"],
			minWage:+legalStructures.filter(d=>d.State===state[1][2])[0]["Set the minimum wage above the low-income threshold of $12 an hour?"],
			noCounselAbortion: +legalStructures.filter(d=>d.State===state[1][2])[0]["Allow women to have an abortion without state-mandated in-person counseling?"],
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
			rape: +femaleCrime.filter(d=>d.Code===state[0])[0]["rape"],
			robbery: +femaleCrime.filter(d=>d.Code===state[0])[0]["robbery"],
			assault: +femaleCrime.filter(d=>d.Code===state[0])[0]["assault"],
			homicide: +femaleCrime.filter(d=>d.Code===state[0])[0]["homicide"],
			// female population
			femalePop: +statesPop.filter(d=>d.state===state[1][2])[0].femalePop,
			// total population
			totPop: +statesPop.filter(d=>d.state===state[1][2])[0].totPop,
			 // party
			 party: stateParty.filter(d=>d.state===state[0])[0].party,
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
	$: rapeNormScale = getNormScale("rape", states)
	$: homicideNormScale = getNormScale("homicide", states)
	$: assaultNormScale = getNormScale("assault", states)
	$: robberyNormScale = getNormScale("robbery", states)

	$: statesIndex = states.map(d=>({...d, 
						titleXNorm: titlexNormScale(d.titlexNum/d.totPop),
						famPlanCentersNorm: famPlanCentersNormScale(d.famPlanCentersNum/d.totPop),
						aProvidersNorm: aProvidersNormScale(d.aProvidersNum/d.totPop),
						famPlanCenterSpendNorm: FamPlanCenterNormScale(d.totalPubDollarsFamPlanCenter/d.totPop),
						abortionDollarsNorm: AbortionDollarsNormScale(d.totalPubDollarsAbortion/d.totPop),
						femaleCrimesNorm: femaleCrimesNormScale(d.femaleCrimes/d.femalePop),
						countiesWoAProvidersNorm: 1-(d.countiesWoAProvidersPct/100),
						CounselBeforeAbortion: 1-(d.noCounselAbortion/100),
						ERANorm: 1-d.ERA,
						minWageNorm: 1-d.minWage,
						parLeaveNorm: 1-d.parLeave,
						unempBenefitsNorm: 1-d.unempBenefits,
						firearmSurrenderNorm: 1-d.firearmSurrender,
						protSHWNorm: 1-d.protSHW,
						// frhSI: 
						// 1-((titlexNormScale((d.titlexNum/d.femalePop))+
						// famPlanCentersNormScale((d.famPlanCentersNum/d.femalePop))+
						// aProvidersNormScale((d.aProvidersNum/d.femalePop))+
						// (1-(d.countiesWoAProvidersPct/100))+
						// FamPlanCenterNormScale((d.totalPubDollarsFamPlanCenter/d.femalePop))+
						// (d.noCounselAbortion)
						// // +
						// // AbortionDollarsNormScale((d.totalPubDollarsAbortion/d.femalePop))+
						// // d.noCounselAbortion
						// )/6),
						frhSI: 
						((titlexNormScale((d.titlexNum/d.femalePop))+
						famPlanCentersNormScale((d.famPlanCentersNum/d.femalePop))+
						aProvidersNormScale((d.aProvidersNum/d.femalePop))+
						(1-(d.countiesWoAProvidersPct/100))+
						FamPlanCenterNormScale((d.totalPubDollarsFamPlanCenter/d.femalePop))+
						(d.noCounselAbortion)
						// +
						// AbortionDollarsNormScale((d.totalPubDollarsAbortion/d.femalePop))+
						// d.noCounselAbortion
						)/6),
						vcSI:
						femaleCrimesNormScale((d.femaleCrimes/d.femalePop)),
						homicideNorm: homicideNormScale(d.homicide/d.femalePop),
						assaultNorm: assaultNormScale(d.assault/d.femalePop),
						robberyNorm: robberyNormScale(d.robbery/d.femalePop),
						rapeNorm: rapeNormScale(d.rape/d.femalePop),
						// lsSI:
						// 1-(d.protSHW + d.firearmSurrender + d.unempBenefits + d.parLeave + d.minWage + d.ERA)/6,
						lsSI:
						(d.protSHW + d.firearmSurrender + d.unempBenefits + d.parLeave + d.minWage + d.ERA)/6,
						yOffset: d.position[1]>=6?-2:0,
						xOffset: d.position[0]>=9?-2:0,
					}
					)
				).map(d=>({...d, 
							deadlyIndex: (d.activeBan + (1-d.frhSI) + d.vcSI + (1-d.lsSI))/4
						// deadlyIndex: (d.activeBan + (1-d.frhSI) + d.vcSI + (1-d.lsSI))/4
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

	// make the last step invisible
	$: if (currentStep===steps.length-1) {
		selectAll(".step-content").style("visibility", "hidden")
	} else if (currentStep!==steps.length-1) {
		selectAll(".step-content").style("visibility", "visible")
	}

</script>
<svelte:window bind:innerWidth bind:outerWidth bind:innerHeight bind:outerHeight />
<main>
	<section>
		<Header {innerWidth}/>
	</section>
	{#if loaded}
		<section>
			<div class="chart">
				<CartogramArc states = {statesIndex} {position} {innerWidth} {innerHeight} {currentStep} {steps}/>
			</div>
			<Scroll bind:value={currentStep}>
				{#each steps as text, i}
				<div class="step" class:active={currentStep === i}>
					<div class="step-content" style="border-left: 4px solid {stepBorderColor};">
					{@html text}
					</div>
				</div>
				{/each}
			</Scroll>
		</section>
	{/if}
	<section>
		<Footer />
	</section>
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
		font-weight: 300;
		max-width: 70vw;
		margin:auto;
		margin-bottom:30px;
		margin-top:100px;
		/* font-family: 'IBM Plex Mono', monospace; */
		/* font-family: 'Inconsolata', monospace; */
		/* font-family: 'Jost', sans-serif; */
		font-family: 'Roboto Flex', sans-serif;
		/* font-family: Georgia, 'Times New Roman', Times, serif; */
	}

	/* h3 {
		font-family: 'Roboto Flex', sans-serif;
		font-weight: 300;
	} */

	p {
		text-align: justify;
		max-width: 70vw;
		margin:auto;
		font-family: 'Jost', sans-serif;
		font-weight: 300;
	}

	.credits {

		/* font-family: 'Roboto', sans-serif; */
		font-family: 'Roboto Flex', sans-serif;
		font-weight: 500;
		/* font-size: calc(7px + 0.4vw); */
		font-size: calc(14px - 0.15vw);
		text-align: center;
		line-height: 1.6em;
		margin-top: 1.5em;
		letter-spacing: 0.1em;
		opacity: 1;
		margin-bottom: 50px;
		/* visibility:hidden; */
		/* transform: translate(10px,93px) */
		/* transform: translate(10px, calc(35px + 3.5vw)) */
		/* font-size: 12px; */
	}


	.chart {
		/* background: whitesmoke; */
		/* width: 400px;
		height: 400px; */
		/* box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2); */
		position: sticky;
		top: 2vh;
		margin: auto;
		height:100vh;
		z-index: 1;
		/* line-height: 100vh; */
		/* vertical-align: middle; */
		/* display:table; */
		/* position: relative; */
		/* height: 100vh */
		/* bottom: 50%; */
	}

  /* .titleElements {
    display:table-cell;
    vertical-align:middle
  } */

  /* Scrollytelling CSS */
  .step {
    height: 150vh;
    display: flex;
    place-items: center;
    justify-content: center;
    max-width: 50vw;
    margin-left:auto;
    margin-right:auto;
	z-index: 2;
    /* z-index: 1000; */
  }

  .step-content {
    /* background: #eeeeee;
    color: #ccc; */
    background: rgb(255, 255, 255, 0.85);
    color: black;
    /* border-radius: 5px; */
    /* border: 2px solid black; */
	/* border-left: 4px solid black; */
    padding: 0.5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
	text-align: justify;
    /* transition: background 500ms ease; */
    box-shadow: 1px 1px 10px rgba(0, 0, 0, 0.2);
    z-index: 10;
	font-family: 'Roboto Flex', sans-serif;
	font-weight: 400;
	font-size: 18px;
	z-index: 2;
  }

  .step.active .step-content {
    background: rgb(255, 255, 255, 1);
    color: black;
  }

  @media (max-width: 640px) {
		main, h1, p, .step {
			max-width: 90vw;
		}
	}
</style>