<template>
  <div class="landing">
    <transition name="head">
      <h1 class="landing-text">Gendering the Smart City</h1>
    </transition>
    <div style="display: grid; grid-template-columns: 1fr 3.5fr 1fr">
      <img
        style="align-self: end; padding-left: 10px"
        src="img/mala.svg"
        alt=""
      />
      <div>
        <div
          style="
            display: grid;
            grid-template-columns: 1fr 1fr;
            text-align: left;
          "
        >
          <div>
            <h4>
              The website showcases excerpts from our research and interviews of
              authors that consist of individuals, non government and community
              based organisations that have functioned relentlessly amidst the
              pandemic to curb the long existing gaps in infrastructure and
              care. The interviewees have worked across communities and in
              several capacities. Their identities are protected and thereby
              represented by pseudonyms:
            </h4>
          </div>
          <div>
            <ul>
              <li>
                - SP is a Dalit activist, writer, social policy researcher, and
                an independent journalist.
              </li>
              <li>
                - LP is an engineer who helps run a community of open data
                enthusiasts.
              </li>
              <li>
                - RJO is an organisation that works on community building and
                policies around abortion and adolescent sexuality.
              </li>
              <li>
                - GH is an organisation that works with informal waste workers.
              </li>
              <li>
                - LL is the cofounder of a women’s rights organisation that
                works issues related to women in the unorganised sector.
              </li>
              <li>
                - KOL is a research and advocacy organisation that works on
                digital economy, digital rights, social justice and democracy.
              </li>
              <li>
                - SDP is a human rights organisation that works for sexual
                minorities, sex workers and people living HIV.
              </li>
              <li>
                - LF is a registered trust that work towards supporting sex
                workers and gender/sexual minorities.
              </li>
            </ul>
          </div>
        </div>
      </div>
      <img
        style="align-self: end; margin-right: 10px"
        src="img/lakshmi.svg"
        alt=""
      />
    </div>
    <h4 style="text-decoration: underline">How to use this site:</h4>
    <h5>
      Click on “Toggle Filters” in order to filter excerpts by interviewee,
      topic, or community. Selecting an interviewee will bring up all their
      inputs, while browsing by topic or community will bring up excerpts
      relevant to each. “Remove All Filters” will bring you back to the default
      view.
    </h5>
  </div>
</template>

<script>
export default {};
</script>

    
<style>
@import url("https://fonts.googleapis.com/css2?family=Lora:wght@400;500&family=Montserrat:wght@300;400&display=swap");
* {
  font-family: "Montserrat", sans-serif;
}
h1 {
  font-size: 4em;
}
.landing {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-content: center;
  background-color: #222222;
}

.head-enter-from {
  opacity: 0;
  transform: translateY(-40px);
}

.head-enter-active {
  transition: all 3s linear;
}

.head-enter-to {
  opacity: 1;
  transform: translateY(0);
}

h4,
h5 {
  color: #dddddd;
  padding-top: 1em;
  padding-bottom: 0.2em;
  margin: 0 auto;
  text-align: left;
  padding: 10px;
}

ul {
  list-style-type: circle;
  color: #cccccc;
  font-size: 14px;
}
li {
  margin: 5px;
}
</style>
