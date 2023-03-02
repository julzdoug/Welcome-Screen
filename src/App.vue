<template>
  <header>
    <div>
      <h1>{{ title }}</h1>
    </div>
    <div>
      <span id="date">{{ currentDate() }}</span>
    </div>
  </header>
  <div class="row">
    <div class="menu">
      <ul v-if="entries && entries.length">
        <li v-for="entry in entries" :key="entry.id">
          <span class="Tims">{{ entry[0] }} Uhr,{{ entry[1].replaceAll("/",".")}}</span>
          <h3 class="Task">{{ entry[2] }}</h3>
          <span class="Task">{{ entry[3] }}</span>
        </li>
      </ul>
      <h1 class="warn" v-else>No Events at the Time available!! </h1>
    </div>
  </div>
  <footer class="footer">
    <img src="./assets/STZH_SEB_Logo.png" alt="">
    <img src="./assets/Opportunity.png" alt="">
    <img src="./assets/SAG_Logo_De.png" alt="">
  </footer>
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

* {
  font-family: "Inter", Arial, Helvetica, sans-serif;
}

.warn {
  top:40%;
}

h1 {
  position: absolute;
  top: 0%;
  max-width: 70%;
  height: 4%;
  left: 5%;
  font-style: normal;
  font-weight: 900;
  font-size: 400%;
  line-height: 75px; 
  color: #323D4A;
}

body {
  background-color: #E8EFF4;

}

#date {
  position: absolute;
  width: 31%;
  min-height: 4%;
  left: 5%;
  top: 5%;
  margin-top: 5%;
  font-style: normal;
  font-weight: 500;
  font-size: 400%;
  line-height: 20%;
  color: #9AA7B1;
}

.menu {
  margin-top: 20%;
  width: 95%;
  right: 5%;
}

li {
  height: 8.6%;
  top: 20%;
  margin-top: 5%;
  background-color: #0F05A0;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
}

.Tims {
  color: #EB5E00;
  font-size: 200%;
  margin-left: 2%;
  line-height: 2%;
  text-justify: auto;

}

.Task {
  color: #FFBFAB;
  font-size: 200%;
  margin-left: 2%;
  line-height: 2%;

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
