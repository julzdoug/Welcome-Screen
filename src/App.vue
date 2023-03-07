<template>
  <!-- staring of the HTML code that will be use on the index.html-->
  <div id="app">

    <!--Importing the Title from the Script Object line 33-->
    <h1>{{ title }}</h1>

    <!--Importing the Date from the script line 45-->
    <h2 id="date" class="date">{{ currentDate() }}</h2>

    <!--Importing the Data of the google sheet in certain order line 36&37-->
    <ul class="menu" v-if="entries && entries.length">

      <!-- without componets -->
      <!--  <li v-for="entry in entries" :key="entry.id">
        <span class="Tims">{{ entry[0] }} Uhr , {{ entry[1].replaceAll("/", ".") }}</span><br>
        <h3 class="Task">{{ entry[2] }}</h3>
        <span class="last">{{ entry[3] }}</span>
      </li>  -->

      <!--With Componets -->
      <li class="entry-item" v-for="entry in entries" :key="entry.id">
        <EventEntry :entry="entry" />
      </li>
    </ul>
    <!--Replace Text If data is empty from the Gsheet-->
    <h3 v-else>No Events at the Time available!! </h3>
    <footer class="footer">
      <!--Logos for the Footer-->
      <img src="./assets/STZH_SEB_Logo.png" alt="">
      <img src="./assets/Opportunity.png" alt="">
      <img src="./assets/SAG_Logo_De.png" alt="">
    </footer>
  </div>
</template>
<!--Start of the script that is use to create the input-->
<script>
import axios from "axios";
import EventEntry from "./components/EventEntry.vue";
export default {
  name: "App",
  components: { EventEntry },
  data() {
    return {
      title: "Welcome to Opportunity",
      sheet_id: "1CT_zjJp_4Sn3JKka9Gn8BihLO3Nr2XY2dLPF7mVkd0I",
      api_token: "AIzaSyBI2CyG50sW8iIPd1RdpsrVA_ShJ8Ox-Rg",
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

    // This filter the data of the google Sheets per date and remove the row past the current date*/
    getData() {
      //Version mit sortierung na aubgelaufenen Datum un daussortierung
      //Version sorted by expired date and sorted out
      //Start with the
      
      axios.get(this.gsheet_url).then((response) => {

        // Get the lines from the API call response
        const rows = response.data.valueRanges[0].values;

        // Get the current date
        const currentDate = new Date();

        // Filter the rows that have a date that is today or later
        const filteredRows = rows.filter(row => {

          // Split the date into its parts
          const dateParts = row[1].split("/");

          // Create a new date from the date parts
          const rowDate = new Date(`${dateParts[2]}-${dateParts[1]}-${dateParts[0]}`);

          // Compare the new date to the current date and return the result
          return rowDate >= currentDate;
        });

        // Sort the filtered rows by date
        this.entries = filteredRows.sort((row1, row2) => {

          // Zerlege die Datumsteile des ersten Datums (uhr Zeit)in row1
          const dateParts1 = row1[1].split("/");

          // Zerlege die Datumsteile des zweiten Datums (Kalender) in row2
          const dateParts2 = row2[1].split("/");

          // Erstelle neue Datumobjekte aus den Datumsteilen beider Zeilen
          const date1 = new Date(`${dateParts1[2]}-${dateParts1[1]}-${dateParts1[0]}`);
          const date2 = new Date(`${dateParts2[2]}-${dateParts2[1]}-${dateParts2[0]}`);

          // Vergleiche die Datumobjekte und gib das Ergebnis zurück
          return date1 - date2;
        });
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
/*Start of the Style Sheet */
/*Import of the google font */
@import url('https://https://fonts.googleapis.com/css2?family=Inter:wght@500;900&display=swap');

/*declaring the behavior of the APP id as parent */
#app {
  font-family: "Inter", Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 30px;
}

/*Style Code for the Title */
h1 {
  font-style: normal;
  font-weight: 900;
  font-size: 62px;
  color: #323D4A;
}

/*Style Code for the body */
body {
  background-color: #E8EFF4;
}

/*Style Code for the Date */
.date {
  font-style: normal;
  font-weight: 500;
  font-size: 62px;
  color: #9AA7B1;
  margin: 0;
}

/*Style Code for the Menu */
.menu {
  padding: 0;

}

/*Style Code for the List */
li {
  padding: 35px 40px;
  margin: 40px 0;
  background-color: #0F05A0;
  list-style: none;
  line-height: 1.3;
  font-size: 28px;
  list-style: none;
}

.Tims {
  color: #EB5E00;
  font-weight: 900;
}

.Task {
  font-size: inherit;
  margin: 0;
  color: #FFBFAB;
  font-weight: 900;
}

.last {
  color: #FFBFAB;
}


/* Style Code for The Footer */
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
}
@media (max-width: 760px) {
  #app {
  font-family: "Inter", Arial, Helvetica, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 10px;
}

/*Style Code for the Title */
h1 {
  font-style: normal;
  font-weight: 600;
  font-size: 10px;
  color: #323D4A;
}

/*Style Code for the body */
body {
  background-color: #E8EFF4;
}

/*Style Code for the Date */
.date {
  font-style: normal;
  font-weight: 300;
  font-size: 10px;
  color: #9AA7B1;
  margin: 0;
}



/*Style Code for the Menu */
.menu {
  padding: 0;

}

/*Style Code for the List */
li {
  padding: 18px 20px;
  margin: 20px 0;
  background-color: #0F05A0;
  list-style: none;
  line-height: 0.8;
  font-size: 8px;
  list-style: none;
}

.Tims {
  color: #EB5E00;
  font-weight: 900;
}

.Task {
  font-size: inherit;
  margin: 0;
  color: #FFBFAB;
  font-weight: 900;
}

.last {
  color: #FFBFAB;
}

/* Style Code for The Footer */
.footer {
  display: flex;
  justify-content:space-evenly;
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  padding: 20px;
  background: #FFF;
}

.footer img {
  height: 30px;
}
}
</style>
