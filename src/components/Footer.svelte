<script>

</script>

<hr style="height:1.5px;border-width:0;color:gray;background-color:lightgrey">
<h4>METHODOLOGY</h4>
<p>
The analysis behind this visual essay makes use of data from the <a href=https://data.guttmacher.org/regions target=__blank>Guttmacher Institute</a>, the <a href=https://crime-data-explorer.app.cloud.gov/pages/explorer/crime target=__blank>Federal Bureau of Investigation (FBI)</a>, the <a href="https://giwps.georgetown.edu/us-index-legal-protections/" target=__blank>Georgetown Institute for Women, Peace and Security (GIWPS)</a> and the <a href=https://www.census.gov/ target=__blank>United States Census Bureau</a>. The indices presented in this piece were calculated as follows:<br><br>

<b>Erosion of Abortion Rights Sub-Index</b>: For a given state, this Sub-Index reflects its Stance on Abortion Rights Given a Potential Roe v. Wade Overturning. Each state can be assigned a score of 0% (no risk of erosion: abortion is legally protected), 33% (low risk of erosion: abortion is not legally protected but the state does not have trigger laws in place to restrict or ban abortion), 66% (high risk of erosion: abortion is not legally protected and the state has trigger laws in place to restrict abortion rights) or 100% (ban: abortion is not legally protected and the state has trigger laws in place to ban abortion).<br>
<b>Reproductive Health Services Sub-Index</b>: For a given state, this Sub-Index reflects its level of accessibility to reproductive health services. To calculate this sub-index, the average of the normalized values for: (1) the per capita number of Title X centers; (2) the per capita number of publicly funded family planning centers; (3) the per capita number of abortion providers; (4) the per capita public expenditure for family planning; (4) whether the state has mandatory in-person counseling for access to abortion; and (5) the percentage of the state's counties that have a known abortion provider is used.<br>
<b>Violent Crime Against Women Sub-Index</b>: This Sub-Index reflects the rate at which women are subjected to violent crime in a given state. To calculate this index, we calculated the average of the (1) number of homicides per 100k women, (2) number of rapes per 100k women, (3) number of aggravated assaults per 100k women and (4) number of robberies per 100k women. This average is then normalized on a scale from 0 to 1.<br>
<b>Lack of Legal Protections</b>: This Sub-Index reflects the degree to which a state offers critical justice-based legal protections for women's rights as outlined by the <a href=https://giwps.georgetown.edu/us-index-legal-protections/ target=__blank>Georgetown Institute for Women, Peace, and Security (GIWPS)</a>. To calculate this sub-index, we scored each state on a scale from 0 to 6 based on whether a state has laws for: 1) protection of all workers from sexual harassment in the workpace, regardless of company size; 2) guaranteed unempoyment benefits for victims of domestic violence, sexual assault, or stalking; 3) mandated parental leave; 4) mandated minimum wage above the low-income threshold; 5) ratified Equal Rights Amendment; and 6) required relinquishment of firearms from abusers subject to domestic violence protective orders. Each state's score is then divided by 6.<br>
<b>Overall Danger Index</b>: The Danger Index represents an average score across all sub-indices, and thus reflects an <i>overall</i> level of danger for each state. It is calculated as follows: Danger Index = ((Erosion of Abortion Rights Sub-Index + (1 - Reproductive Health Services Sub-Index) + Violent Crime Against Women Sub-Index + (1-Lack of Legal Protections Sub-Index))/4.<br><br>

Each of the analysis, design, and development elements of this project were conducted in JavaScript. This webpage, along with the data visualizations, is built with the <a href=https://svelte.dev/ target=__blank>Svelte</a> framework, and the <a href=https://d3js.org/ target=__blank>D3.js</a> JavaScript library. The scrollytelling functionality was achieved using Svelte's implementation of <a href=https://github.com/russellgoldenberg/scrollama target=__blank>Scrollama.js</a> developed by <a href="https://twitter.com/codenberg?lang=en" target=__blank>Russel Goldenberg</a> and with the help of <a href=https://www.connorrothschild.com/ target=__blank>Connor Rothschild</a>'s awesome <a href=https://www.connorrothschild.com/post/svelte-scrollytelling target=__blank>tutorial</a>. The code behind this application is freely available on  <a href="" target=__blank>Github</a>.
</p>

<style>

    hr {
        margin-top: 70px;
        max-width: 50vw;
    }
    .specialWord {
        /* background-color: blueviolet; */
        color: white;
        font-weight: 700;
    }

    .specialWordWrap {
        background-color: #730f71;
        padding-right: 20px;
        padding-left: 20px;
        display: inline-block;
        transform:skew(-.312rad)
    }

    .specialWordWrapPar {
        background-color: #730f71;
        padding-right: 5px;
        padding-left: 5px;
        display: inline-block;
        transform:skew(-.312rad);
        color:white;
        border:1px solid white

    }

    .specialPar {
        text-align: left;
		max-width: 50vw;
        font-size: 18px;
		margin:auto;
		/* font-family: 'Jost', sans-serif; */
        font-family: 'Roboto Flex', sans-serif;
		font-weight: 400;
    }

    h1 {
		color: black;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 400;
        /* font-style: thin; */
		max-width: 60vw;
		margin:auto;
		margin-bottom:30px;
		margin-top:70px;
        /* color: "white"; */
		/* font-family: 'IBM Plex Mono', monospace; */
		/* font-family: 'Inconsolata', monospace; */
		/* font-family: 'Jost', sans-serif; */
		font-family: 'Roboto Flex', sans-serif;
		/* font-family: Georgia, 'Times New Roman', Times, serif; */
	}

    .questions {
        /* font-weight: 300; */
        font-style:italic;
        /* text-decoration: underline; */
    }

    .titleContainer {
        /* background-color: black; */
    }

	h3 {
        color: black;
		font-family: 'Roboto Flex', sans-serif;
		font-weight: 300;
	}

	p {
		text-align: justify;
		max-width: 50vw;
        font-size: 14px;
		margin:auto;
		/* font-family: 'Jost', sans-serif; */
        font-family: 'Roboto Flex', sans-serif;
		font-weight: 300;
        /* line-height: normal; */
	}

    a {
        text-decoration:underline; 
        color:black
    }

	.credits {

        color: black;
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

    .scrollButton {
      background-color: white;
      /* width: 140px; */
      height:27px;
      color:black;
      font-weight:700;
      font-size: 13px;
      font-family: 'Roboto Flex', sans-serif;
      text-transform: uppercase;
      border: 1.7px solid black;
      cursor: pointer;
  }

  .scrollButton:hover {
      background-color: #730f71;
      color:white;
      border: 1.7px solid #3c083c;
  }

	@media (max-width: 640px) {
		hr, h4, p {
			max-width: 90vw;
		}
	}
</style>