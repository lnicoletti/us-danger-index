<script>
    import * as animateScroll from "svelte-scrollto";

    function revertFrameOne() {
        animateScroll.scrollTo({element: '#lastStep'})
    }

</script>

<div class="titleContainer">
    <h1>The <br><div class="specialWordWrap"><span class="specialWord">Most Dangerous</span></div><br> U.S. States to reside in<br>if you are not a cis-gender man</h1>
    <h3>A Tool Designed with Female Assigned at Birth (AFAB) and Femme Folx' Safety in Mind</h3>
    <div class="credits">
        by <a href='https://www.carolinecullinan.com/' target='_blank'><b>CAROLINE CULLINAN</b></a><br> and <a href='https://www.leonardonicoletti.com/' target='_blank'><b>LEONARDO NICOLETTI</b></a><br>
    </div>
</div>
<p>As leaked by Politico on May 2, 2022, the Supreme Court vote to overturn Roe v. Wade further exemplifies institutional harm towards folx who do not identify as cisgender men.  In recognizing a persistent attack on the rights of people of marginalized genders, we created the following tool.  Specifically focused on the safety of female assigned at birth (AFAB) and femme folx, an overall danger index has been created to identify US states that have been rendered more or less dangerous for those who are not cisgender men.  In creating this tool, we hope to highlight and bring greater transparency to the fact that across all US states, institutional systems of oppression perpetuate gender-based harm.</p><br>
<p>Interested in understanding what went into this overall danger index?  Scroll through the following story to find out.  Alternatively, Click <button class="scrollButton" on:click={() => revertFrameOne()}>here</button> to jump to the bottom of the screen and access our tool immediately.</p><br>
<p><span class="questions">Firstly, what constitutes danger?  Additionally, how does danger vary across states?</span></p><br><br>

<div class="specialPar">In creating the overall danger index for our tool, several sub-indices, related to:<div class="specialWordWrapPar">the erosion of abortion rights</div>; <div class="specialWordWrapPar">a lack of reproductive health services</div>; <div class="specialWordWrapPar">violent crimes committed against women</div>; and <div class="specialWordWrapPar">the lack of state legal protections</div>, were considered.</div>

<style>

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
        font-size: 18px;
		margin:auto;
		/* font-family: 'Jost', sans-serif; */
        font-family: 'Roboto Flex', sans-serif;
		font-weight: 400;
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
		main, h1, p, .specialPar {
			max-width: 90vw;
		}
	}
</style>