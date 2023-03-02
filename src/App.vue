<template>
    <div id="app">
      <h1>{{ title }}</h1>
        <span id="date">{{ currentDate() }}</span>
        <ul class="menu" v-if="entries && entries.length">
        <dd v-for="entry in entries" :key="entry.id">
          <span class="Tims">{{ entry[0] }} Uhr,{{ entry[1].replaceAll("/",".")}}</span>
          <h3 class="Task">{{ entry[2] }}</h3>
          <span class="last">{{ entry[3] }}</span>
        </dd>
      </ul>
      <h1 v-else>No Events at the Time available!! </h1>
  <footer class="footer">
    <img src="./assets/STZH_SEB_Logo.png" alt="">
    <img src="./assets/Opportunity.png" alt="">
    <img src="./assets/SAG_Logo_De.png" alt="">
  </footer>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      title: "Welcome to Opportunity",
      sheet_id: "1CR1UKN0LAPNs6lWbfA2gBI2FazmWdVSFIzIwi5TG5Z4",
      api_token: "AIzaSyA-qeDXOhEeQDA0vQf7LgkF7DQtGnAtmAU",
      entries: [],
    };
  },
  computed: {
    gsheet_url() {
      return `https://sheets.googleapis.com/v4/spreadsheets/${this.sheet_id}/values:batchGet?ranges=A2%3AE100&valueRenderOption=FORMATTED_VALUE&key=${this.api_token}`;
    },
  },
  methods: {
    currentDate() {
      const current = new Date();
      const date = `${current.getDate()}.${current.getMonth() + 1}.${current.getFullYear()}`;
      return date;
    },
    /* getData() {

      this.entries = [
        ["17:08", "08/09/2023", "abschlussfeier", "erfolgreich vorbei!!"],
        ["08:08", "17/09/2023", "aufräumen", "Bitte alle Helfen"],
      ];
    }, */
    getData() {
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values;
      });
    },
    refreshData() {
      this.currentDate();
      this.getData();
    },
  },
  mounted() {
    this.refreshData();
    setInterval(this.refreshData, 18000000);
  },
};
</script>
<style>
@import url('https://https://fonts.googleapis.com/css2?family=Inter:wght@500;900&display=swap%22');
#app {
  font-family: "Inter", Arial, Helvetica, sans-serif;
  
 }



h1 {
  position: absolute;
  left: 8%;
  font-style: normal;
  font-weight: 900;
  font-size: 400%;
  line-height: 75px; 
  color: #323D4A;
}

body {
  background-color: #E8EFF4;
  /* width: 1080px; */
  /* height:1920px; */
  /* display: list-item; */
}

#date {
  position: absolute;
  top:9%;
  left: 8%;
  font-style: normal;
  font-weight: 500;
  font-size: 400%;
  line-height: 20%;
  color: #9AA7B1;
}

.menu {
  position: absolute;
  top:10%;
  display: flexbox;
  width: 88%;
  line-height: 3%;
}

dd {
/*   top: 20%; */
  padding:5%;
  margin-top: 5%;
  background-color: #0F05A0;
  list-style: none;
  }

.Tims {
  color: #EB5E00;
  font-size: 200%;
  text-justify: auto;
  font-weight: bolder;
  
}

.Task {
  color: #FFBFAB;
  font-size: 200%;
  font-weight: bold;
}
.last {
  color: #FFBFAB;
  font-size: 200%;
  line-height: 1.3;
margin-bottom: 5%;
}

.footer {
  display: flex;
  justify-content: space-between;
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  padding: 40px;
  background: #FFF;

}

.footer img {
  height: 50px;
}</style>
