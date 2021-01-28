<template>
  <div class="container">
    <section class="filters">
      <button @click="toggle" style="margin: 1em">Toggle Filters</button>
      <button @click="removeFilters" style="margin: 1em">
        Remove All Filters
      </button>

      <transition name="fade">
        <div class="filter-container" v-if="active">
          <div class="filter">
            <h3>Interviewees</h3>
            <div
              class="form-group"
              v-for="item in Authors"
              v-bind:key="item.id"
            >
              <label class="form-check-label" :for="item.id">
                <input
                  type="checkbox"
                  v-model="user.authorCollection"
                  :id="item.name"
                  :value="item.name"
                  @change="convertFilters"
                />
                {{ item.name }}
              </label>
            </div>
          </div>

          <div class="filter">
            <h3>Topics</h3>
            <div class="form-group" v-for="item in Titles" v-bind:key="item.id">
              <label class="form-check-label" :for="item.id">
                <input
                  type="checkbox"
                  v-model="user.titleCollection"
                  :id="item.name"
                  :value="item.name"
                  @change="convertFilters"
                />{{ item.name }}</label
              >
            </div>
          </div>

          <div class="filter">
            <h3>Communities</h3>
            <div
              class="form-group"
              v-for="item in Communities"
              v-bind:key="item.id"
            >
              <label class="form-check-label" :for="item.id">
                <input
                  type="checkbox"
                  v-model="user.communityCollection"
                  :id="item.name"
                  :value="item.name"
                  @change="convertFilters"
                />{{ item.name }}
              </label>
            </div>
          </div>
        </div>
      </transition>
    </section>

    <section class="filtered-content">
      <div class="article-wrapper" v-for="item in displayData" :key="item.ID">
        <div v-if="item.type === 'paragraph'">
          <div class="title">
            <h6 class="topic">{{ item.topic }} - {{ item.author }}</h6>
          </div>
          <div class="content easeout">
            <p class="content" v-bind:class="item.color">
              {{ item.content }}
            </p>
          </div>
        </div>

        <div v-if="item.type === 'pullquote'">
          <div class="content">
            <blockquote>"{{ item.content }}"</blockquote>

            <div class="author">
              <h5 style="text-align: right; padding-right: 3rem" class="author">
                - {{ item.author }}
              </h5>
            </div>
          </div>
        </div>

        <div v-if="item.type === 'image1'">
          <img class="image-item" :src="getImgUrl(item.imgname1)" alt="image" />
        </div>

        <div v-if="item.type === 'image2'">
          <img class="image-item" :src="getImgUrl(item.imgname2)" alt="image" />
        </div>
      </div>
    </section>
    <!-- <img class="cats" src="https://placekitten.com/g/200/300" alt="jacket" /> -->
  </div>
</template>

    <script>
import DataFrame from "dataframe-js";
import anime from "animejs/lib/anime.es.js";

// const images = require.context("@/assests/img/", false, /\.png$|\.jpg$/);

export default {
  mounted() {
    this.fetchData();
  },
  methods: {
    removeFilters() {
      window.location.reload(false);
    },
    convertFilters() {
      let filters = JSON.parse(JSON.stringify(this.user));

      // filter by author

      let d = [];
      for (let i = 0; i < filters.authorCollection.length; i++) {
        let v = filters.authorCollection[i];
        let k = "author";
        let o = { filterCriteria: k, filterOption: v };
        d.push(o);
      }

      // filter by topics

      for (let i = 0; i < filters.titleCollection.length; i++) {
        let v = filters.titleCollection[i];
        let k = "topic";
        let o = { filterCriteria: k, filterOption: v };
        d.push(o);
      }

      // filter by communities

      for (let i = 0; i < filters.communityCollection.length; i++) {
        let v = filters.communityCollection[i];
        let k = "communities";
        let o = { filterCriteria: k, filterOption: v };
        d.push(o);
      }

      console.log(d);
      this.constructFilterDF(d);
      this.dataFiltering();
    },
    constructFilterDF(data) {
      this.filterDf = new DataFrame(data, ["filterCriteria", "filterOption"]);
      // this.filterDf.show();
      this.filterArray = this.filterDf.toArray();
      console.log(this.filterArray);
    },
    dataFiltering() {
      this.displayData = [];
      for (let i = 0; i < this.filterArray.length; i++) {
        let colName = this.filterArray[i][0];
        let rowValue = this.filterArray[i][1];
        if (this.displayData.length === 0) {
          this.displayData = this.df.filter((row) =>
            row.get(colName).toString().includes(rowValue)
          );
        } else {
          let dftemp = this.df.filter((row) =>
            row.get(colName).toString().includes(rowValue)
          );
          this.displayData = this.displayData.union(dftemp);
          this.displayData = this.displayData.dropDuplicates();
        }
      }
      this.displayData = this.displayData.toCollection();
      this.animate();
    },
    handleSubmit() {
      alert(JSON.stringify(this.user));
    },
    toggle() {
      this.active = !this.active;
      // this.animate();
      if (this.active === true) {
        anime({
          targets: ".filtered-content",
          opacity: 0.4,
          duration: 200,
          easing: "easeInOutSine",
        });
      } else if (this.active === false) {
        anime({
          targets: ".filtered-content",
          opacity: 1.0,
          duration: 500,
          easing: "linear",
        });
      }
    },
    getAuthors() {
      let d = [];
      let arr = this.df.unique("author").toArray();
      for (let i = 0; i < arr.length; i++) {
        let o = { name: arr[i][0] };
        d.push(o);
      }
      this.Authors = d;
    },
    getTopics() {
      let d = [];
      let arr = this.df.unique("topic").toArray();
      for (let i = 0; i < arr.length; i++) {
        let o = { name: arr[i][0] };
        d.push(o);
      }
      this.Titles = d;
    },
    getCommunities() {
      let d = [];
      let arr = this.df.unique("communities").toArray();
      for (let i = 0; i < arr.length; i++) {
        let o = { name: arr[i][0] };
        d.push(o);
      }
      this.Communities = d;
    },
    async fetchData() {
      this.df = await DataFrame.fromJSON("data4.json");
      this.df.show();
      this.getAuthors();
      this.getTopics();
      this.getCommunities();
      this.displayData = this.df.toCollection();
      this.animate();
    },
    getImgUrl(img) {
      let x = "../assets/img/" + img + ".jpg";
      x = "img/" + img + ".jpg";
      console.log(x);
      return x;
    },
    animate() {
      anime({
        targets: ".article-wrapper",
        translateY: 250,
        duration: 200,
        direction: "reverse",
        easing: "easeInOutSine",
      });
      // let e = document.getElementsByClassName("article-wrapper");
      // console.log(e.length);
      // for (let i = 0; i < e.length; i++) {
      //   console.log("hi");
      //   e[i].style.transition = "all 1.0s linear 0s";
      //   e[i].style.transform = "translateY(0px)";
      //   e[i].style.opacity = "1";
      // }
    },
  },

  data() {
    return {
      yellow: true,
      displayData: [],
      active: false,
      Authors: [],
      Titles: [],
      Communities: [],
      user: {
        authorCollection: [],
        titleCollection: [],
        communityCollection: [],
        imgCollection: [],
        tagCollection: [],
        color: [],
      },
    };
  },

  name: "Mainpage",
};
</script>

  
    <style>
@import url("https://fonts.googleapis.com/css2?family=Lora:wght@400;500&family=Montserrat:wght@300;400&display=swap");

* {
  font-family: "Montserrat", sans-serif;
}

body::-webkit-scrollbar {
  width: 12px; /* width of the entire scrollbar */
}

body::-webkit-scrollbar-track {
  background: #111111; /* color of the tracking area */
}

body::-webkit-scrollbar-thumb {
  background-color: #333333; /* color of the scroll thumb */
  border-radius: 20px; /* roundness of the scroll thumb */
  border: 1px double #FFC966; /* creates padding around scroll thumb */
}

.container {
  display: flex;
  flex-direction: column;
  grid-gap: 0 10%;
  justify-items: center;
  position: relative;
  background-color: #111111;
}

h3 {
  margin: 40px 0 5px;
  padding-left: 1em;
  text-align: left;
  border-bottom: 2px double #dddddd;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

section {
  color: #eeeeee;
}

.filter-container {
  position: absolute;
  z-index: 10;
  width: 100%;
  margin: auto;
  display: flex;
  justify-content: space-around;
  opacity: 0.85;
  background-color: #333333;
  border-bottom: 2px double #cccccc;
}

.filtered-content {
  overflow: scroll;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  text-align: left;
  align-items: flex-start;
}

.filtered-content::after {
  display: block;
  margin: 10px;
  flex: 999 999 auto;
}

.filtered-content > .article-wrapper:nth-child(5n + 1) {
  width: 23%;
  padding: 1em;
}
.filtered-content > .article-wrapper:nth-child(5n + 2) {
  width: 25%;
}
.filtered-content > .article-wrapper:nth-child(5n + 3) {
  width: 35%;
}
.filtered-content > .article-wrapper:nth-child(5n + 4) {
  width: 30%;
}
.filtered-content > .article-wrapper:nth-child(5n + 5) {
  width: 39%;
}

p {
  font-size: 14px;
}

/* Hide scrollbar for Chrome, Safari and Opera */
.filtered-content::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.filtered-content {
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}

@media only screen and (max-width: 1081px) {
  .filtered-content > div:nth-child(2n + 1) {
    width: 60%;
  }
  .filtered-content > div:nth-child(2n + 2) {
    width: 70%;
  }
  .filtered-content > div:nth-child(2n + 3) {
    width: 55%;
  }
}

button {
  background-color: #2c3e50;
  color: #eeeeee;
  border: none;
  padding: 5px 20px;
  border-radius: 20px; /* roundness of the scroll thumb */
  border: 1px double #FFC966; /* creates padding around scroll thumb */
}

p {
  border-left: #eeeeee 1px solid;
  padding-left: 1em;
}

h6 {
  padding-left: 1em;
}

img {
  width: 100%;
}

.pink {
  border-left: #ea62d8 2px double;
}

.yellow {
  border-left: #eacb4f 2px double;
}

.orange {
  border-left: #ea7e4f 1px double;
}

.purple {
  border-left: #9b42f4 1px double;
}

.red {
  border-left: #ef1a4c 1px double;
}

.brown {
  border-left: #512818 1px double;
}

.green {
  border-left: #7dba47 1px double;
}

.cyan {
  border-left: #23d7eb 1px double;
}

.article-wrapper {
  transition: background-color 0.5s cubic-bezier(0, 0, 0.32, 1.31);
  transition: color 0.5s cubic-bezier(0, 0, 0.32, 1.31) 0.2s;
  transition: transform 0.5s cubic-bezier(0, 0, 0.32, 1.31);
  margin: 2em;
  padding: 10px;
  /* padding: 2em; */
}

.article-wrapper:hover {
  transform: scale(1.04);
  transform: translateY(-3px);
  background-color: #cccccc;
  color: #111111;
  box-shadow: 5px 5px 10px 1px rgba(255, 255, 255, 0.15);
}

.filter {
  display: flex;
  flex-direction: column;
}

.form-group {
  align-self: flex-start;
}

blockquote {
  font-family: "Lora", serif;
  font-weight: 500;
  font-size: 18px;
  text-align: right;
  padding-right: 10px;
  border-right: double 2px #eeeeee;
}

para-enter-from {
  opacity: 0;
  translate: rotate(40px);
}

para-enter-active {
  transition: all 4s ease-in;
}

para-enter-to {
  opacity: 1;
  translate: rotate(140px);
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.2s cubic-bezier(0, 0, 0, 1);
}

.fade-enter-from,
.fade-leave-to {
  transform: translateY(-250px);
}
</style>


