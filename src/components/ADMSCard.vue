<template>
  <div>
    <b-col>
      <b-card
        :img-src="getImgUrl(imgsrc)"
        img-alt="Icon"
        img-top
        tag="article"
        style="max-width:17rem; height:500px; font-size:13px; border: none"
        class="mb-4"
        :footer="name"
      >
        <b-card-text style="padding:0">
          <div class="" style="display:grid; grid-template-columns:1fr 1fr">
            <p>
              <span> <strong>Year:</strong> </span> {{ year }}
            </p>
            <p>{{ imgsrc }}</p>
            <p>
              <span> <strong>Area:</strong> </span> {{ area }}
            </p>
            <p>
              <span> <strong>Jurisdiction:</strong> </span> {{ jurisdiction }}
            </p>
            <p>
              <span> <strong>Purpose:</strong> </span> {{ purpose }}
            </p>
          </div>
        </b-card-text>

        <div>
          <b-button
            class="modal-button"
            v-b-modal="'card-details'"
            squared
            variant="dark"
            v-on:click="modalClick"
            :value="id"
            >More</b-button
          >
        </div>
      </b-card>
    </b-col>
    <!-- <button class="btn btn-primary"> test </button> -->
  </div>
</template>

<script>
export default {
  props: ["name", "area", "year", "jurisdiction", "purpose", "id", "imgsrc"],
  methods: {
    modalClick() {
      this.$emit("card-callback", this.id);
    },
    getImgUrl(img) {
      var images = require.context("../../public/", false, /\.png$/);
      return images("./" + img + ".png");
    }
  }
};
</script>

<style>
.dark-card {
  background-color: #000000;
}
</style>
