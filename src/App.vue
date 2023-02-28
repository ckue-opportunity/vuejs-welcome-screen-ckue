<template>
  <div id="app">
    <h1 class="site-title">{{ title }}</h1>
    <span class="site-description">{{ currentDate }}</span>

    <!-- entry list -->
    <!-- v-if will only be shown if there are entries available -->
    <ul v-if="entries" class="entry-list">
      <li class="entry-item" v-for="entry in entries" :key="entry.id">
        <span class="entry-daytime">{{entry[0]}} Uhr, {{entry[1].replaceAll("/", ".")}}</span><br />
        <h3 class="entry-title">{{entry[2]}}</h3>
        <span class="entry-description">{{entry[3]}}</span><br />
      </li>
    </ul>

    <!-- ELSE  -->
    <h1 v-else>Keine Events zur Zeit vorhanden!</h1>

    <!-- footer -->
    <footer class="footer">
      <img
        src="./assets/STZH_SEB_Logo.png"
        alt="Stadt Zürich Soziale Betriebe Logo"
      >
      <img
        src="./assets/Opportunity.png"
        alt="Opportunity Zürich Logo"
      >
      <img
        src="./assets/SAG_Logo_De.png"
        alt="Stiftung Arbeitsgestaltung Logo"
      >
    </footer>
  </div>
</template>

<script>
import axios from "axios"; // axios is a library for making HTTP requests to the backend

export default {
  name: "App",
  data() {
    return {
      title: "Welcome to Opportunity",
      /* Tugce's settings:
        sheet_id: "1CR1UKN0LAPNs6lWbfA2gBI2FazmWdVSFIzIwi5TG5Z4",
        api_token: "AIzaSyA-qeDXOhEeQDA0vQf7LgkF7DQtGnAtmAU",
      */
      sheet_id: "1iLd4VMoTUFTxnaZNEEuEsqGgYtGIHXAkl-EK5yR8bcs",
      api_token: "AIzaSyDeEt8NzZpi4z3pbn9vXElA8NqEgXRvWKI",
      entries: [],
      currentDate: "",
    };
  },
  computed: {
    // computed properties are like data properties, but with a method combined and it gets executed automatically, instead of calling a function explicitly
    gsheet_url() {
      return `https://sheets.googleapis.com/v4/spreadsheets/${this.sheet_id}/values:batchGet?ranges=A2%3AE100&valueRenderOption=FORMATTED_VALUE&key=${this.api_token}`;
    },
  },
  methods: {
    getData() {
      axios.get(this.gsheet_url).then((response) => {
        this.entries = response.data.valueRanges[0].values;
      });
    },
    updateCurrentDate() {
      let today = new Date();
      const currentDate = `${today.getDate()}.${today.getMonth() + 1}.${today.getFullYear()}`;
      this.currentDate = currentDate;
    },
    refreshData() {
      this.updateCurrentDate();
      this.getData();
    },
  },
  mounted() {
    this.refreshData(); // get first initial data and then wait for the next
    setInterval(this.refreshData, 18000000); // wait 30mins for next update (1000 * 60 * 30)
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@500;900&display=swap");

body {
  background: #e8eff4;
}

#app {
  font-family: "Inter", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #323d4a;
  margin: 60px;
}

.site-title {
  font-size: 62px;
  font-weight: 900;
  margin: 80px 0 20px 0;
}

.site-description {
  font-size: 62px;
  color: #9aa7b1;
  font-weight: 500;
  margin: 0;
}

.entry-list {
  padding-left: 0;
}

.entry-item {
  padding: 35px 40px;
  margin: 40px 0;
  background: #0f05a0;
  font-size: 28px;
  line-height: 1.3;
  list-style: none;
}

.entry-daytime {
  color: #eb5e00;
  font-weight: 900;
}

.entry-title {
  font-size: inherit;
  margin: 0;
  color: #ffbfab;
  font-weight: 900;
}

.entry-description {
  color: #ffbfab;
}

.footer {
  display: flex;
  justify-content: space-between;
  box-sizing: border-box;
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  padding: 40px;
  background: #fff;
}

.footer img {
  height: 50px;
}
</style>
