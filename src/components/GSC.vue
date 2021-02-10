<template>
  <div class="container">
    <section class="filters">
      <div>
        <button @click="toggle" style="margin: 1em">Toggle Filters</button>
      </div>
      <div>
        <button @click="removeFilters" style="margin: 1em">
          Remove All Filters
        </button>
      </div>

      <div class="dropdown">
        <button
          @click="toggleHowTo"
          style="margin: 1em; background-color: #512772"
        >
          How to Use this Site
        </button>
        <div v-if="howto" class="toggle-howto">
          <h4>How to use this site:</h4>
          <h5>
            Click on “Toggle Filters” in order to filter excerpts by
            interviewee, topic, or community. Selecting an interviewee will
            bring up all their inputs, while browsing by topic or community will
            bring up excerpts relevant to each. “Remove All Filters” will bring
            you back to the default view.
          </h5>
        </div>
      </div>
      <transition name="fade">
        <div class="filter-container" v-if="active">
          <div class="filter">
            <h3>Interviewees</h3>
            <div
              class="form-group"
              v-for="item in Authors"
              v-bind:key="item.id"
            >
              <label class="form-check-label" :for="item.id" :class="item.name">
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
            <blockquote :class="'pull-' + item.author">
              "{{ item.content }}"
            </blockquote>

            <div class="author">
              <h5 style="text-align: right; padding-right: 3rem" class="author">
                - {{ item.author }}
              </h5>
            </div>
          </div>
        </div>

        <div v-if="item.type === 'image1'">
          <figure>
            <a :href="item.link1" target="_blank">
              <img
                class="image-item"
                :src="getImgUrl(item.imgname1)"
                alt="image"
                :class="'image-' + item.author"
              />
            </a>
            <figcaption>
              {{ item.caption1 }}
              <a :href="item.link1" target="_blank"> [source] </a>
            </figcaption>
          </figure>
        </div>

        <div v-if="item.type === 'image2'">
          <figure>
            <a :href="item.link2" target="_blank">
              <img
                class="image-item"
                :src="getImgUrl(item.imgname2)"
                alt="image"
                :class="'image-' + item.author"
              />
            </a>
            <figcaption>
              {{ item.caption2 }}
              <a :href="item.link2" target="_blank"> [source] </a>
            </figcaption>
          </figure>
        </div>

        <div v-if="item.type === 'video'">
          <iframe
            :class="'image-' + item.author"
            width="350"
            height="197"
            src="https://www.youtube.com/embed/EGkEL1atdhc"
            frameborder="0"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen
          ></iframe>
        </div>
      </div>
    </section>
    <!-- <img class="cats" src="https://placekitten.com/g/200/300" alt="jacket" /> -->
  </div>
</template>

    <script>
import DataFrame from "dataframe-js";
import anime from "animejs/lib/anime.es.js";
// import main from'@/assets/css/main.css'
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
        let k = "tags";
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

      // console.log(d);
      this.constructFilterDF(d);
      this.dataFiltering();
    },
    constructFilterDF(data) {
      this.filterDf = new DataFrame(data, ["filterCriteria", "filterOption"]);
      // this.filterDf.show();
      this.filterArray = this.filterDf.toArray();
      // console.log(this.filterArray);
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
    toggleHowTo() {
      this.howto = !this.howto;
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
      let checkArray = [];
      let arr = this.df.unique("tags").toArray();
      for (let i = 0; i < arr.length; i++) {
        let r = arr[i][0].split(",");
        for (let j = 0; j < r.length; j++) {
          let elem = r[j].trim();
          if (checkArray.indexOf(elem) === -1) {
            checkArray.push(elem);
            let o = { name: elem };
            d.push(o);
          }
        }
      }
      this.Titles = d;
    },
    getCommunities() {
      let d = [];
      let checkArray = [];
      let arr = this.df.unique("communities").toArray();
      for (let i = 0; i < arr.length; i++) {
        let r = arr[i][0].split(",");
        for (let j = 0; j < r.length; j++) {
          let elem = r[j].trim();
          if (checkArray.indexOf(elem) === -1) {
            checkArray.push(elem);
            let o = { name: elem };
            d.push(o);
          }
        }
      }
      this.Communities = d;
    },
    async fetchData() {
      this.df = await DataFrame.fromJSON("data8.json");
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
      // console.log(x);
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
      howto: false,
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
</style>


