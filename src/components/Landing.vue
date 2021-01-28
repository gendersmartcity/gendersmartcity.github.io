<template>
  <div class="landing">
    <transition name="head">
      <h1 class="landing-text">Gendering the Smart City</h1>
    </transition>
    <div style="display: grid; grid-template-columns: 1fr 3.5fr 1fr">
      <img
        style="align-self: end; padding-left: 10px; max-height: 400px"
        src="img/mala.svg"
        alt=""
      />
      <div>
        <div
          style="
            display: grid;
            grid-template-columns: 1fr 1fr;
            text-align: left;
            grid-gap: 100px;
          "
        >
          <div style="align-self: center">
            
            <h4 style="text-align: left; border-right: 2px double #ea62d8">
              The website showcases excerpts from our interviews of individuals
              working with NGOs and CBOs that worked tirelessly through the
              pandemic to bridge considerable gaps in infrastructure and care.
              The interviewees have worked across communities and in several
              capacities. Their identities are protected and thereby represented
              by pseudonyms:
            </h4>
            <div class="arrow">
              <div class="chevron"></div>
              <div class="chevron"></div>
              <div class="chevron"></div>
            </div>
          </div>
          <div style="align-self: center">
            <ul>
              <li>
                - <span class="bold-span">SP</span> is a Dalit activist, writer,
                social policy researcher, and an independent journalist.
              </li>
              <li>
                - <span class="bold-span">LP</span> is an engineer who helps run
                a community of open data enthusiasts.
              </li>
              <li>
                - <span class="bold-span">RJO</span> is an organisation that
                works on community building and policies around abortion and
                adolescent sexuality.
              </li>
              <li>
                - <span class="bold-span">GH</span> is an organisation that
                works with informal waste workers.
              </li>
              <li>
                - <span class="bold-span">LL</span> is the cofounder of a
                women’s rights organisation that works issues related to women
                in the unorganised sector.
              </li>
              <li>
                - <span class="bold-span">KOL</span> is a research and advocacy
                organisation that works on digital economy, digital rights,
                social justice and democracy.
              </li>
              <li>
                - <span class="bold-span">SDP</span> is a human rights
                organisation that works for sexual minorities, sex workers and
                people living HIV.
              </li>
              <li>
                - <span class="bold-span">LF</span> is a registered trust that
                work towards supporting sex workers and gender/sexual
                minorities.
              </li>
            </ul>
          </div>
        </div>
      </div>

      <img
        style="align-self: end; margin-right: 10px; max-height: 400px"
        src="img/lakshmi.svg"
        alt=""
      />
    </div>
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

.bold-span {
  color: #ea62d8;
  font-weight: 800;
}

.arrow {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.chevron {
  position: absolute;
  width: 20px;
  height: 8px;
  opacity: 0;
  transform: scale(0.3);
  animation: move-chevron 3s ease-out infinite;
}

.chevron:first-child {
  animation: move-chevron 3s ease-out 1s infinite;
}

.chevron:nth-child(2) {
  animation: move-chevron 3s ease-out 2s infinite;
}

.chevron:before,
.chevron:after {
  content: "";
  position: absolute;
  top: 0;
  height: 100%;
  width: 50%;
  background: #2c3e50;
}

.chevron:before {
  left: 0;
  transform: skewY(30deg);
}

.chevron:after {
  right: 0;
  width: 50%;
  transform: skewY(-30deg);
}

@keyframes move-chevron {
  25% {
    opacity: 1;
  }
  33.3% {
    opacity: 1;
    transform: translateY(30px);
  }
  66.6% {
    opacity: 1;
    transform: translateY(50px);
  }
  100% {
    opacity: 0;
    transform: translateY(90px) scale(0.5);
  }
}
</style>
