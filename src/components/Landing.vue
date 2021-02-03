<template>
  <div class="landing">
    <div class="header-holder">
      <img class="mobile-img-1" src="img/mala.svg" alt="" />
      <img class="mobile-img-2" src="img/lakshmi.svg" alt="" />
      <h1 class="landing-text">Gendering the Smart City</h1>
    </div>
    <div class="front-container">
      <img class="front-img" src="img/mala.svg" alt="" />
      <div>
        <div class="front-text">
          <div style="align-self: center">
            <h4 style="text-align: left; border-right: 2px double #ea62d8">
              The website showcases excerpts from our interviews of individuals
              working with NGOs and CBOs that worked tirelessly through the
              pandemic to bridge considerable gaps in infrastructure and care.
              The interviewees have worked across communities and in several
              capacities. Their identities are protected and thereby represented
              by pseudonyms:
            </h4>
            <div class="downArrow bounce">
              <img
                width="50"
                height="50"
                alt=""
                style="filter: blur(1px) invert(59%) sepia(63%) saturate(1748%) hue-rotate(277deg) brightness(93%) contrast(96%);"
                src="data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiA/PjwhRE9DVFlQRSBzdmcgIFBVQkxJQyAnLS8vVzNDLy9EVEQgU1ZHIDEuMS8vRU4nICAnaHR0cDovL3d3dy53My5vcmcvR3JhcGhpY3MvU1ZHLzEuMS9EVEQvc3ZnMTEuZHRkJz48c3ZnIGVuYWJsZS1iYWNrZ3JvdW5kPSJuZXcgMCAwIDMyIDMyIiBoZWlnaHQ9IjMycHgiIGlkPSLQodC70L7QuV8xIiB2ZXJzaW9uPSIxLjEiIHZpZXdCb3g9IjAgMCAzMiAzMiIgd2lkdGg9IjMycHgiIHhtbDpzcGFjZT0icHJlc2VydmUiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyIgeG1sbnM6eGxpbms9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiPjxwYXRoIGQ9Ik0yNC4yODUsMTEuMjg0TDE2LDE5LjU3MWwtOC4yODUtOC4yODhjLTAuMzk1LTAuMzk1LTEuMDM0LTAuMzk1LTEuNDI5LDAgIGMtMC4zOTQsMC4zOTUtMC4zOTQsMS4wMzUsMCwxLjQzbDguOTk5LDkuMDAybDAsMGwwLDBjMC4zOTQsMC4zOTUsMS4wMzQsMC4zOTUsMS40MjgsMGw4Ljk5OS05LjAwMiAgYzAuMzk0LTAuMzk1LDAuMzk0LTEuMDM2LDAtMS40MzFDMjUuMzE5LDEwLjg4OSwyNC42NzksMTAuODg5LDI0LjI4NSwxMS4yODR6IiBmaWxsPSIjMTIxMzEzIiBpZD0iRXhwYW5kX01vcmUiLz48Zy8+PGcvPjxnLz48Zy8+PGcvPjxnLz48L3N2Zz4="
              />
            </div>
          </div>
          <div style="align-self: center">
            <ul>
              <li>
                - <span class="bold-span yellow-span">SP</span> is a Dalit
                activist, writer, social policy researcher, and an independent
                journalist.
              </li>
              <li>
                - <span class="bold-span pink-span">KOL</span> is a research and
                advocacy organisation that works on digital economy, digital
                rights, social justice and democracy.
              </li>
              <li>
                - <span class="bold-span orange-span">LP</span> is an engineer
                who helps run a community of open data enthusiasts.
              </li>
              <li>
                - <span class="bold-span purple-span">GH</span> is an
                organisation that works with informal waste workers.
              </li>
              <li>
                - <span class="bold-span red-span">LL</span> is the cofounder of
                a women’s rights organisation that works issues related to women
                in the unorganised sector.
              </li>
              <li>
                - <span class="bold-span brown-span">RJO</span> is an
                organisation that works on community building and policies
                around abortion and adolescent sexuality.
              </li>
              <li>
                - <span class="bold-span green-span">SDP</span> is a human
                rights organisation that works for sexual minorities, sex
                workers and people living HIV.
              </li>
              <li>
                - <span class="bold-span cyan-span">LF</span> is a registered
                trust that work towards supporting sex workers and gender/sexual
                minorities.
              </li>
            </ul>
          </div>
        </div>
      </div>

      <img class="front-img" src="img/lakshmi.svg" alt="" />
    </div>
  </div>
</template>

<script>
export default {};
</script>

    
<style>
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

.front-container {
  display: grid;
  grid-template-columns: 1fr 3.5fr 1fr;
}
.front-text {
  display: grid;
  grid-template-columns: 1fr 1fr;
  text-align: left;
  grid-gap: 100px;
}

.front-img {
  align-self: end;
  padding-left: 10px;
  max-height: 400px;
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

.pink-span {
  color: #ea62d8;
}

.yellow-span {
  color: #eacb4f;
}

.orange-span {
  color: #ea7e4f;
}

.purple-span {
  color: #9b42f4;
}

.red-span {
  color: #ef1a4c;
}

.brown-span {
  color: #933f3f;
}

.green-span {
  color: #7dba47;
}

.cyan-span {
  color: #23d7eb;
}
</style>
