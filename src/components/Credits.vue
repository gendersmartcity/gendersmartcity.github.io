<template>
  <div class="credits">
    <div style="display: flex">
      <img src="img/db.png" alt="" class="logo-img db" />
      <div>
        <p>Design Beku</p>
        <p>
          Team: Padmini Ray Murray,
          <a href="https://prtysh.github.io">Pratyush Raman</a>,
          <a href="https://senjutisangia.myportfolio.com/">Senjuti Sangia</a>
        </p>
      </div>
    </div>
    <div>
      <p>
        With thanks to: Cynthia Stephen, Geetha Menon,
        <a href="https://hasirudala.in/">Hasirudala</a>,
        <a href="https://hiddenpocketscollective.org/">Hidden Pockets</a>,
        <a href="https://itforchange.net/">IT for Change</a>,
        <a href="http://sangama.org/">Sangama</a>,
        <a href="https://www.solidarityfoundation.in/">Solidarity Foundation</a
        >,
        <a href="https://thejeshgn.com/">Thejesh GN.</a>
      </p>
    </div>
    <div style="display: flex">
      <p>
        This microsite is part of the &lsquo;Gendering the Smart City&rsquo;
        project funded by AHRC&rsquo;
      </p>
      <img src="img/ahrc.png" alt="" class="logo-img ahrc" />
    </div>
  </div>
</template>

<script>
export default {};
</script>

    
<style>
.credits {
  display: flex;
  background-color: #000000;
  color: #888888;
}

.logo-img {
  height: 70px;
  width: auto;
  object-fit: contain;
  margin: auto;
  padding: 0px 20px;
}


@media only screen and (max-width: 1081px) and (orientation: portrait) {
  .credits {
    flex-direction: column;
  }
}
</style>
