<script>

</script>

<hr style="height:1.5px;border-width:0;color:gray;background-color:lightgrey">
<h4>METHODOLOGY</h4>
<p>
    The analysis behind this visual essay makes use of <a href="" target=__blank>this data</a>, <a href="" target=__blank>this data</a>, <a href="" target=__blank>this data</a>, and <a href="" target=__blank>this data</a>. The indices presented in this piece was calculated as follows:<br><br>
    <b>Erosion of Abortion Rights Sub-Index</b>: Method here.<br>
    <b>Lack of Reproductive Health Services</b>: Method here.<br>
    <b>Violent Crime Against Women</b>: Method here.<br>
    <b>Lack of Legal Protections</b>: Method here.<br>
    <b>Overall Danger Index</b>: The danger index represents an average score across all sub-indices.
    <br><br>
    Each of the analysis, design, and development elements of this project were conducted in JavaScript. This webpage, along with the data visualizations, is built with the <a  href="" target=__blank>Svelte</a> framework, and the <a  href="" target=__blank>D3.js</a> JavaScript library. The scrollytelling functionality was achieved using the Svelte's implementation of <a  href="" target=__blank>Scrollama.js</a> developed by <a  href="" target=__blank>Russel Goldenberg</a>. The behind this application is freely available on <a  href="" target=__blank>Github</a>.
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