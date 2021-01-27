<template>
  <div style="margin:auto">
    <div style="margin:auto; display:flex;">
      <b-dropdown id="dropdown-1" text="Purpose" class="m-md-2 dropdown">
        <!-- <b-dropdown-item>Facial Recognition</b-dropdown-item> -->
        <div>
          <b-form-checkbox-group
            v-model="selected"
            :options="purposeFilters"
            class="mb-3"
            value-field="name"
            text-field="item"
            disabled-field="notEnabled"
            @change="updateSelected"
          >
          </b-form-checkbox-group>
        </div>
        <b-dropdown-divider></b-dropdown-divider>

        <!-- <b-dropdown-item active>Crime Data Analysis</b-dropdown-item> -->
        <!-- <b-dropdown-item disabled>Sentiment Analysis</b-dropdown-item> -->
      </b-dropdown>

      <b-dropdown id="dropdown-2" text="Area" class="m-md-2 dropdown">
        <div>
          <b-form-checkbox-group
            v-model="selected"
            :options="areaFilters"
            class="mb-3"
            value-field="name"
            text-field="item"
            disabled-field="notEnabled"
            @change="updateSelected"
          >
          </b-form-checkbox-group>
        </div>
        <b-dropdown-divider></b-dropdown-divider>
      </b-dropdown>

      <b-dropdown id="dropdown-3" text="Year" class="m-md-2 dropdown">
        <div>
          <b-form-checkbox-group
            v-model="selected"
            :options="yearFilters"
            class="mb-3"
            value-field="name"
            text-field="item"
            disabled-field="notEnabled"
            @change="updateSelected"
          >
          </b-form-checkbox-group>
        </div>
        <b-dropdown-divider></b-dropdown-divider>
      </b-dropdown>

      <b-dropdown id="dropdown-4" text="Name" class="m-md-2 dropdown">
        <div>
          <b-form-checkbox-group
            v-model="selected"
            :options="nameFilters"
            class="mb-3"
            value-field="name"
            text-field="item"
            disabled-field="notEnabled"
            @change="updateSelected"
          >
          </b-form-checkbox-group>
        </div>
        <b-dropdown-divider></b-dropdown-divider>
      </b-dropdown>

      <b-dropdown id="dropdown-5" text="Jurisdiction" class="m-md-2 dropdown">
        <div>
          <b-form-checkbox-group
            v-model="selected"
            :options="jurisdictionFilters"
            class="mb-3"
            value-field="name"
            text-field="item"
            disabled-field="notEnabled"
            @change="updateSelected"
          >
          </b-form-checkbox-group>
        </div>
        <b-dropdown-divider></b-dropdown-divider>
      </b-dropdown>
    </div>

    <div class="">
      <b-row align-v="center" align-h="center">
        <Card
          v-on:card-callback="constructModal"
          v-for="card in filteredCards"
          :key="card.ID"
          :name="card.Name"
          :area="card.Area"
          :year="card['Year of Deployment']"
          :jurisdiction="card.Jurisdiction"
          :purpose="card.Purpose"
          :id="card.ID"
          :imgsrc="card['Icon Name']"
        >
        </Card>
      </b-row>
    </div>

    <b-modal id="card-details" size="xl" hide-footer>
      <div></div>
      <div class="d-block text-left">
        <!-- style="display:grid; grid-template-columns:1fr 1fr" -->
        <b-row>
          <b-col>
            <p><strong>Name </strong>:&nbsp; {{ modalContent["Name"] }}</p>
            <p><strong>Area </strong>:&nbsp; {{ modalContent["Area"] }}</p>
            <p>
              <strong>Purpose </strong>:&nbsp; {{ modalContent["Purpose"] }}
            </p>
            <p>
              <strong>Jurisdiction </strong>:&nbsp;
              {{ modalContent["Jurisdiction"] }}
            </p>
            <p>
              <strong>Year of Deployment </strong>:&nbsp;
              {{ modalContent["Year of Deployment"] }}
            </p>
            <p>
              <strong>Algorithm or Model Type </strong>:&nbsp;
              {{ modalContent["Algorithm or Model Type"] }}
            </p>
            <p>
              <strong>Manner of Procurement </strong>:&nbsp;
              {{ modalContent["Manner of Procurement"] }}
            </p>
            <p>
              <strong>Developed For or Requested By </strong>:&nbsp;
              {{ modalContent["Developed For or Requested By"] }}
            </p>
            <p>
              <strong>Developed By </strong>:&nbsp;
              {{ modalContent["Developed By"] }}
            </p>
            <p>
              <strong>Proposed or Implemented </strong>:&nbsp;
              {{ modalContent["Proposed or Implemented"] }}
            </p>
            <p>
              <strong>Databases Relied On Training or Matching </strong>:&nbsp;
              {{ modalContent["Databases Relied On Training or Matching"] }}
            </p>

            <p v-if="publicDocLinks != ''">
              <strong> Public Documentation:&nbsp;</strong>
              <a v-for="link in publicDocLinks" :key="link" :href="link"
                >link</a
              >
            </p>
            <p v-if="modalLinks != ''">
              <strong> News Reports:&nbsp; </strong>
              <a v-for="link in modalLinks" :key="link" :href="link"
                >link&nbsp; &nbsp;</a
              >
            </p>
          </b-col>

          <div class="col">
            <b-img
              left
              style="width:100%"
              :src="getImgUrl(iconName)"
              fluid
              alt="icon"
            ></b-img>
          </div>
        </b-row>
      </div>
    </b-modal>
  </div>
</template>

<script>
import DataFrame from "dataframe-js";
// import DataFrame, { Row } from 'dataframe-js';
import Card from "@/components/ADMSCard.vue";
// import pimg1 from "../../public/pimp1.png"
export default {
  props: ["name", "area", "nme", "year", "jurisdiction", "purpose", "id", "imgsrc"],
  components: {
    Card: Card
  },

  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData(){
      this.df = await DataFrame.fromJSON("adms_array6.json");
      // this.df.cast("Year of Deployment", Number);
      this.df = this.df.fillMissingValues("NA",['Manner of Procurement']);
      this.constructModal();
    },
    updateSelected() {
      let s = JSON.parse(JSON.stringify(this.selected));
      var d = [];
      // console.log(Object.values(s));
      for (let i = 0; i < s.length; i++) {
        let v = Object.values(s[i]).[0];
        let k = Object.keys(s[i]).[0];
        // console.log(k);
        let o = {filterCriteria: k, filterOption: v};
        // console.log(o);
        d.push(o);
      }
      this.constructFilterDF(d);
      this.cardFiltering();
    },
    constructFilterDF(data) {
      this.filterDf = new DataFrame(data, ["filterCriteria", "filterOption"]);
      // this.filterDf.show();
      this.filterArray = this.filterDf.toArray();
      console.log(this.filterArray);

    },
    cardFiltering(){
      // this.displayCards = this.df.filter(row => row.get('Jurisdiction').includes('Delhi'));
      // this.df.filter(row => row.get('Jurisdiction').includes('Delhi')).show();
      this.displayCards=[];
      for (let i = 0; i < this.filterArray.length; i++) {
        console.log(typeof(this.filterArray[i][0]));
        console.log(typeof(this.filterArray[i][1]));
        let colName = this.filterArray[i][0];
        let rowValue = this.filterArray[i][1];
        if(this.displayCards.length === 0){
          this.displayCards = this.df.filter(row => row.get(colName).toString().includes(rowValue));
        } else {
          let dftemp = this.df.filter(row => row.get(colName).toString().includes(rowValue));
          this.displayCards = this.displayCards.union(dftemp);
          this.displayCards = this.displayCards.dropDuplicates();
        }

        // this.displayCards = this.df.filter(row => row.get(this.filterArray[i][0]).search(this.filterArray[i][1]));
      }
      this.displayCards.select("Name", "Year of Deployment", "ID").show();
      this.filteredCards = this.displayCards.toCollection();
    },
    constructModal(value){
      // console.log(value);
      let modalContentDf = this.df.chain(row => row.get('ID') == value)
      // modalContentDf.select("ID","Purpose").show();
      this.modalContent = modalContentDf.toDict();
      let modalLinks = this.modalContent["News Reports"];
      this.modalLinks = modalLinks[0].split("; ");
      // console.log(this.modalLinks);
      let publicDocLinks = this.modalContent["Public Documentation"];
      this.publicDocLinks = publicDocLinks[0].split("; ");
      this.iconName =  this.modalContent["Icon Name"];
      console.log(this.iconName);
      // this.iconLink="../../public/pimg4.png";
      // this.iconLink="https://placekitten.com/g/200/300";
      // console.log(this.iconLink[0]);

    },
    getImgUrl(img) {
    var images = require.context('../../public/', false, /\.png$/);
    return images('./' + img + ".png");
  }
  },
  data() {
    return {df: [],
      displayCards: [],
      filterDf: [],
      selected: [],
      filteredCards: [],
      modalContent:[],
      modalLinks:[],
      publicDocLinks:[],
      iconName:"pimg4",
      purposeFilters: [
      {item: "Facial Recognition",  name: { Purpose: "Facial Recognition" }},
      {item: "Social Media Surveillance",  name: { Purpose: "Social Media Surveillance" }},
      {item: "Predictive Policing",  name: { Purpose: "Predictive Policing" }},
      {item: "Crime Data Analytics",  name: { Purpose: "Crime Data Analytics" }},
      {item: "Sentiment Analysis",  name: { Purpose: "Sentiment Analysis" }},
      {item: "Counter-Disinformation",  name: { Purpose: "Counter-Disinformation" }},
      {item: "Beneficiary Identification for Farm Loan Waiver ",  name: { Purpose: "Beneficiary Identification for Farm Loan Waiver " }},
      {item: "Farm Loan Waiver Identification",  name: { Purpose: "Farm Loan Waiver Identification" }},
      {item: "Health Inusrance Fraud Analaysis (Ayushman Bharat)",  name: { Purpose: "Health Inusrance Fraud Analaysis (Ayushman Bharat)" }},
      {item: "Digital Contact Tracing",  name: { Purpose: "Digital Contact Tracing" }},
      {item: "Tax Fraud Analytics",  name: { Purpose: "Tax Fraud Analytics" }},
      {item: "Pension Fraud Analytics",  name: { Purpose: "Pension Fraud Analytics" }},
      {item: "Beneficiary Eligibility",  name: { Purpose: "Beneficiary Eligibility" }},
      {item: "Biometric Identification",  name: { Purpose: "Biometric Identification" }},
      {item: "Identification",  name: { Purpose: "Identification" }},
      {item: "Beneficiary Eligibility for Welfare Schemes",  name: { Purpose: "Beneficiary Eligibility for Welfare Schemes" }},
      {item: "Voter Identification",  name: { Purpose: "Voter Identification" }},
      {item: "School Allocation",  name: { Purpose: "School Allocation" }},
      {item: "Student Performance",  name: { Purpose: "Student Performance" }},
      {item: "Facial Recognition for Student Attendance",  name: { Purpose: "Facial Recognition for Student Attendance" }},
      {item: "Credit Risk Scoring",  name: { Purpose: "Credit Risk Scoring" }},
      {item: "Automated Number Plate Recognition",  name: { Purpose: "Automated Number Plate Recognition" }},
      {item: "Worker Performance",  name: { Purpose: "Worker Performance" }},
      {item: "Urban Planning",  name: { Purpose: "Urban Planning" }}
      ],
      areaFilters: [
      {item: "Policing and Surveillance",  name: { Area: "Policing and Surveillance" }},
      {item: "Welfare",  name: { Area: "Welfare" }},
      {item: "Health",  name: { Area: "Health" }},
      {item: "Taxation",  name: { Area: "Taxation" }},
      {item: "Elections",  name: { Area: "Elections" }},
      {item: "Education",  name: { Area: "Education" }},
      {item: "Banking and Finance",  name: { Area: "Banking and Finance" }},
      {item: "Transport",  name: { Area: "Transport" }},
      {item: "Sanitation",  name: { Area: "Sanitation" }},
      ],
      yearFilters: [
      {item: "2009",  name: { "Year of Deployment": "2009" }},
      {item: "2013",  name: { "Year of Deployment": "2013" }},
      {item: "2014",  name: { "Year of Deployment": "2014" }},
      {item: "2015",  name: { "Year of Deployment": "2015" }},
      {item: "2016",  name: { "Year of Deployment": "2016" }},
      {item: "2017",  name: { "Year of Deployment": "2017" }},
      {item: "2018",  name: { "Year of Deployment": "2018" }},
      {item: "2019",  name: { "Year of Deployment": "2019" }},
      {item: "2020",  name: { "Year of Deployment": "2020" }}
      ],
      nameFilters: [
      {item: "AI Vision",  name: { "Name": "AI Vision" }},
      {item: "FaceTagr",  name: { "Name": "FaceTagr" }},
      {item: "NeoFace",  name: { "Name": "NeoFace" }},
      {item: "AMBIS",  name: { "Name": "AMBIS" }},
      {item: "Mumbai City Surveillance Project",  name: { "Name": "Mumbai City Surveillance Project" }},
      {item: "Punjab Artificial Intelligence System (PAIS)",  name: { "Name": "Punjab Artificial Intelligence System (PAIS)" }},
      {item: "ABHED",  name: { "Name": "ABHED" }},
      {item: "TSCOP",  name: { "Name": "TSCOP" }},
      {item: "Trinetra",  name: { "Name": "Trinetra" }},
      {item: "Prahaar",  name: { "Name": "Prahaar" }},
      {item: "Automated Facial Recognition System",  name: { "Name": "Automated Facial Recognition System" }},
      {item: "Maharashtra Big Data Analysis Tool",  name: { "Name": "Maharashtra Big Data Analysis Tool" }},
      {item: "CMAPS",  name: { "Name": "CMAPS" }},
      {item: "COGNOS",  name: { "Name": "COGNOS" }},
      {item: "PRAHAAR ",  name: { "Name": "PRAHAAR " }},
      {item: "Social Media Lab",  name: { "Name": "Social Media Lab" }},
      {item: "AASMA (Advanced Application for Social Media Analytics)",  name: { "Name": "AASMA (Advanced Application for Social Media Analytics)" }},
      {item: "PhotoDNA",  name: { "Name": "PhotoDNA" }},
      {item: "FACTS (Fraud Analytics Control and Tracking System)",  name: { "Name": "FACTS (Fraud Analytics Control and Tracking System)" }},
      {item: "Aarogya Setu",  name: { "Name": "Aarogya Setu" }},
      {item: "Project Insight",  name: { "Name": "Project Insight" }},
      {item: "Samagra Vedika",  name: { "Name": "Samagra Vedika" }},
      {item: "KALIA",  name: { "Name": "KALIA" }},
      {item: "Integrated Social Protection Delivery Platform",  name: { "Name": "Integrated Social Protection Delivery Platform" }},
      {item: "PMUY",  name: { "Name": "PMUY" }},
      {item: "Aadhaar",  name: { "Name": "Aadhaar" }},
      {item: "UID",  name: { "Name": "UID" }},
      {item: "MNREGA",  name: { "Name": "MNREGA" }},
      {item: "PDS",  name: { "Name": "PDS" }},
      {item: "Aadhaar",  name: { "Name": "Aadhaar" }},
      {item: "Big Data Environment",  name: { "Name": "Big Data Environment" }},
      {item: "Makkal",  name: { "Name": "Makkal" }},
      {item: "Samagra",  name: { "Name": "Samagra" }},
      {item: "NERPAP",  name: { "Name": "NERPAP" }},
      {item: "Real Time Authentication of Voter Identity",  name: { "Name": "Real Time Authentication of Voter Identity" }},
      {item: "Human Efficiency Tracking System",  name: { "Name": "Human Efficiency Tracking System" }}
      ],
      jurisdictionFilters: [
      {item: "New Delhi",  name: { Jurisdiction: "New Delhi" }},
      {item: "Chennai",  name: { Jurisdiction: "Chennai" }},
      {item: "Chitoor",  name: { Jurisdiction: "Chitoor" }},
      {item: "Surat",  name: { Jurisdiction: "Surat" }},
      {item: "Maharasthra",  name: { Jurisdiction: "Maharasthra" }},
      {item: "Vijaywada",  name: { Jurisdiction: "Vijaywada" }},
      {item: "Jaipur",  name: { Jurisdiction: "Jaipur" }},
      {item: "Mumbai",  name: { Jurisdiction: "Mumbai" }},
      {item: "Punjab",  name: { Jurisdiction: "Punjab" }},
      {item: "Uttarakhand",  name: { Jurisdiction: "Uttarakhand" }},
      {item: "Gurgaon",  name: { Jurisdiction: "Gurgaon" }},
      {item: "Rajasthan",  name: { Jurisdiction: "Rajasthan" }},
      {item: "Telangana",  name: { Jurisdiction: "Telangana" }},
      {item: "Nagpur",  name: { Jurisdiction: "Nagpur" }},
      {item: "Uttar Pradesh",  name: { Jurisdiction: "Uttar Pradesh" }},
      {item: "Gujarat",  name: { Jurisdiction: "Gujarat" }},
      {item: "Kolkata",  name: { Jurisdiction: "Kolkata" }},
      {item: "Bihar",  name: { Jurisdiction: "Bihar" }},
      {item: "Across India",  name: { Jurisdiction: "Across India" }},
      {item: "Kerala",  name: { Jurisdiction: "Kerala" }},
      {item: "Maharashtra",  name: { Jurisdiction: "Maharashtra" }},
      {item: "Haryana",  name: { Jurisdiction: "Haryana" }},
      {item: "Jharkhand",  name: { Jurisdiction: "Jharkhand" }},
      {item: "Orissa",  name: { Jurisdiction: "Orissa" }},
      {item: "Karnataka",  name: { Jurisdiction: "Karnataka" }},
      {item: "Odisha",  name: { Jurisdiction: "Odisha" }},
      {item: "Tamil Nadu",  name: { Jurisdiction: "Tamil Nadu" }},
      {item: "Madhya Pradesh",  name: { Jurisdiction: "Madhya Pradesh" }},
      {item: "Telangana, Bihar",  name: { Jurisdiction: "Telangana, Bihar" }},
      {item: "Kompally Municipality, Telangana",  name: { Jurisdiction: "Kompally Municipality, Telangana" }},
      {item: "Andhra Pradesh",  name: { Jurisdiction: "Andhra Pradesh" }},
      {item: "Coimbatore",  name: { Jurisdiction: "Coimbatore" }},
      {item: "Behrampur",  name: { Jurisdiction: "Behrampur" }},
      {item: "Kolkata ",  name: { Jurisdiction: "Kolkata " }},
      {item: "Pimpri Chinchwad",  name: { Jurisdiction: "Pimpri Chinchwad" }},
      {item: "Chandigarh",  name: { Jurisdiction: "Chandigarh" }},
      {item: "Indore",  name: { Jurisdiction: "Indore" }},
      ],
    };
  },
};
</script>

<style lang="scss" scoped></style>
