<script>
    export let innerWidth;
    import * as animateScroll from "svelte-scrollto";

    function revertFrameOne() {
        animateScroll.scrollTo({element: '#lastStep', duration:4000});
    }

    $: titleFontSize = innerWidth>900?60:innerWidth>750?50:innerWidth>640?40:innerWidth>460?30:23//style="font-size:{titleFontSize}"
    $: subTitleFontSize = innerWidth>640?30:20//style="font-size:{titleFontSize}"

</script>

<div class="titleContainer">
    <!-- <h1 style="font-size:{titleFontSize}px">The <br><div class="specialWordWrap"><span class="specialWord">Most</span></div><br><div class="specialWordWrap"><span class="specialWord">Dangerous</span></div><br> U.S. States to reside in</h1> -->
    <h1 style="font-size:{titleFontSize}px">
        The <br>
        <div class="specialWordWrapSquare"><span class="specialWord">M</span></div><div class="specialWordWrapSquare"><span class="specialWord">o</span></div><div class="specialWordWrapSquare"><span class="specialWord">s</span></div><div class="specialWordWrapSquare"><span class="specialWord">t</span></div>
            <br>
        <div class="specialWordWrapSquare"><span class="specialWord">D</span></div><div class="specialWordWrapSquare"><span class="specialWord">a</span></div><div class="specialWordWrapSquare"><span class="specialWord">n</span></div><div class="specialWordWrapSquare"><span class="specialWord">g</span></div><div class="specialWordWrapSquare"><span class="specialWord">e</span></div><div class="specialWordWrapSquare"><span class="specialWord">r</span></div><div class="specialWordWrapSquare"><span class="specialWord">o</span></div><div class="specialWordWrapSquare"><span class="specialWord">u</span></div><div class="specialWordWrapSquare"><span class="specialWord">s</span></div><br> U.S. States to reside in</h1>
    <h2 style="font-size:{subTitleFontSize}px">(if you are not a cis-gender man)</h2>
    <h3>A Tool Designed with Female Assigned at Birth (AFAB) and Femme Folx' Safety in Mind</h3>
    <div class="credits">
        by <a href='https://www.carolinecullinan.com/' target='_blank'><b>CAROLINE CULLINAN</b></a><br> and <a href='https://www.leonardonicoletti.com/' target='_blank'><b>LEONARDO NICOLETTI</b></a><br>
    </div>
</div>
<p>As leaked by <a href="https://www.politico.com/news/2022/05/02/supreme-court-abortion-draft-opinion-00029473" target=__blank>Politico</a> on May 2, 2022, the Supreme Court vote to overturn Roe v. Wade further exemplifies institutional harm towards folx who do not identify as cisgender men.  In recognizing a persistent attack on the rights of people of marginalized genders, we created the following tool.  Specifically focused on the safety of female assigned at birth (AFAB) and femme folx, an overall danger index has been created to identify US states that have been rendered more or less dangerous for those who are not cisgender men.  In creating this tool, we hope to highlight and bring greater transparency to the fact that across all US states, institutional systems of oppression perpetuate gender-based harm.</p><br>
<p>Interested in understanding what went into this overall danger index?  Scroll through the following story to find out.  Alternatively, click <button class="scrollButton" on:click={() => revertFrameOne()}>here</button> to jump to the bottom of the screen and access our tool immediately.</p><br>
<!-- <p><span class="questions">Firstly, what constitutes danger?  Additionally, how does danger vary across states?</span></p><br>

<div class="specialPar">In creating the overall danger index for our tool, several sub-indices, related to:<div class="specialWordWrapPar" style="background-color:darkred">the erosion of abortion rights</div>; <div class="specialWordWrapPar" style="background-color:rgb(21, 127, 60)">a lack of reproductive health services</div>; <div class="specialWordWrapPar" style="background-color:rgb(200, 10, 34)">violent crimes committed against women</div>; and <div class="specialWordWrapPar" style="background-color:rgb(58, 59, 83)">the lack of state legal protections</div>, were considered.</div> -->

<style>

    .specialWord {
        /* background-color: blueviolet; */
        color: white;
        font-weight: 700;
        /* center vertically */
        /* position: relative; */
        /* top: 10%;
        -ms-transform: translateY(-10%);
        transform: translateY(-10%); */
    }

    .specialWordWrap {
        background-color: #730f71;
        padding-right: 20px;
        padding-left: 20px;
        display: inline-block;
        /* transform:skew(-.312rad) */
    }
    .specialWordWrapSquare {
        background-color: #4a095c;
        padding-right: 2px;
        padding-left: 2px;
        display: inline-block;
        border: 1.5px solid white;
        width:6vw;
        /* height:6vw */
        /* transform:skew(-.312rad) */
    }

    .specialWordWrapPar {
        /* background-color: #730f71; */
        padding-right: 5px;
        padding-left: 5px;
        display: inline-block;
        /* transform:skew(-.312rad); */
        color:white;
        border:1px solid white;
        border-radius: 2.5px;

    }

    .specialPar {
        text-align: left;
		max-width: 50vw;
        font-size: 18px;
		margin:auto;
		/* font-family: 'Jost', sans-serif; */
        font-family: 'Roboto Flex', sans-serif;
		font-weight: 300;
    }

    h1 {
		color: black;
		text-transform: uppercase;
		/* font-size: 4em; */
        /* font-size: calc(30px + 8vw); */
        /* font-size: 4vw; */
		font-weight: 600;
        /* font-style: thin; */
		/* max-width: 60vw; */
		margin:auto;
		margin-bottom:30px;
		margin-top:70px;
        margin-bottom: 0px;
        /* color: "white"; */
		/* font-family: 'IBM Plex Mono', monospace; */
		/* font-family: 'Inconsolata', monospace; */
		/* font-family: 'Jost', sans-serif; */
		font-family: 'Roboto Flex', sans-serif;
		/* font-family: Georgia, 'Times New Roman', Times, serif; */
	}
    h2 {
        color: black;
		font-family: 'Roboto Flex', sans-serif;
		font-weight: 400;
        margin-top: 0px;
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
      display: inline-block;
      padding: 2px 1px;
      margin:0;
      /* width: 140px; */
      /* height:20px; */
      color:black;
      font-weight:400;
      font-size: 18px;
      font-family: 'Roboto Flex', sans-serif;
      /* text-transform: uppercase; */
      border: 2px solid white;
      text-decoration: underline;
      cursor: pointer;
  }

  .scrollButton:hover {
      /* background-color: #ccc; */
      /* color:black; */
      text-decoration: none;
      border: 2px solid black;
  }

	@media (max-width: 640px) {
		main, h1, p, .specialPar {
			max-width: 90vw;
		}
	}
</style>