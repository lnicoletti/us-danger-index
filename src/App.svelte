<script>
	// export let name;
	import { onMount } from "svelte";
	import { csv, json, csvParseRows} from 'd3';
	let position = new Map;
	let femaleCrime=[];
	let abbrv=[];
	let femaleHealth=[];
	let crimeProtLaws=[];
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

	onMount(async () => {

		await Promise.all([
			csv("/data/crime.csv"),
			csv("/data/stateAbbrv.csv"),
			csv("/data/femaleHealth.csv"),
			csv("/data/crimeProtLaws.csv"),
			json("/data/statesPop.json")
		])
		.then((datasets)=>{
			femaleCrime = datasets[0];
			abbrv = datasets[1];
			femaleHealth = datasets[2];
			crimeProtLaws = datasets[3];
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
			protUIB:crimeProtLaws.filter(d=>d.State===state[1][2])[0]["Does State Law Provide Unemployment Insurance Benefits to Domestic Violence Victims?"]==="No"?0:1,
			protERL:crimeProtLaws.filter(d=>d.State===state[1][2])[0]["Does the State Have an Employment Rights Law for Victims of Domestic Violence?"]==="No"?0:1,
			protGCRPL:crimeProtLaws.filter(d=>d.State===state[1][2])[0]["Does the State Have a General Crime Victim Protection Law?"]==="No"?0:1,
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

	$: console.log(states)

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

</script>

<main>
	<!-- <h1>Hello {name}!</h1> -->
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>