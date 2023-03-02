<template>
  <div id="app">
    <h1 class="site-title">{{ title }}</h1>
    <span class="site-description">{{ currentDate }}</span>

    <!-- entry list -->
    <ul v-if="entries" class="entry-list">
      <li v-for="entry in entries" :key="entry.id" class="entry-item">
        <span class="entry-daytime">{{ entry[1] }} {{ entry[0] }} Uhr</span><br />
        <h3 class="entry-title">{{ entry[2] }}</h3>
        <span class="entry-description">{{ entry[3] }}</span><br />
      </li>
    </ul>

    <!-- default text is only shown if the list of entries is null or undefined -->
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
export default {
  name: "App",
  data() {
    return {
      title: "Welcome to Opportunity",
      currentDate: "",
      entries: []
    };
  },
  methods: {
    getData() {
      this.entries = [
        // Time, Date, Title, Description
        ["17:00",	"08/09/2022",	"Abschlussfeier",	"Erfolgreich vorbei!!!"],
        ["19:00",	"09/09/2022",	"Aufräumen",	"Bitte alle helfen"],
        ["19:30",	"10/09/2022",	"Ausschlafen",	"Gute Nacht..."]
      ];
    },
    updateCurrentDate() {
      let today = new Date();
      this.counter++;
      this.currentDate = `${today.getDate()}.${today.getMonth() + 1}.${today.getFullYear()}`;
    },
    refreshData() {
      this.updateCurrentDate();
      this.getData();
    },
  },
  mounted() {
    this.refreshData(); // get first initial data and then wait for the next
    setInterval(this.refreshData, 60000); // wait one minute for next update (1000 * 60)
  }
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
