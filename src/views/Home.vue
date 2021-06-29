<template>
  <div>
    <nav-bar></nav-bar>
    <h2 class="font-bold text-purple-700 text-center mb-5 text-2xl mt-5">Today's matches</h2>
    <div class="flex justify-center items-center overflow-x-scroll px-5">
      <table class="table-auto border-separate border border-purple-800">
        <thead>
          <tr>
            <th class="border border-purple-600 text-center p-1">Date</th>
            <th class="border border-purple-600 text-center p-1">Home team</th>
            <th class="border border-purple-600 text-center p-1">Away team</th>
            <th class="border border-purple-600 text-center p-1">Competition</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="match in todayMatches" :key="match.id">
            <td class="border border-purple-600 text-center p-1">{{ getHumanDate(match.utcDate) }}</td>
            <td class="border border-purple-600 text-center p-1">{{ match.homeTeam.name }}</td>
            <td class="border border-purple-600 text-center p-1">{{ match.awayTeam.name }}</td>
            <td class="border border-purple-600 text-center p-1">{{ match.competition.name }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import NavBar from "@/components/NavBar.vue";
import MatchItem from "@/components/MatchItem.vue";
import env from "@/env.js";
import moment from "moment"
export default {
  name: "Home",
  data() {
    return {
      todayMatches: [],
    };
  },
  components: {
    NavBar,
    MatchItem,
  },
  beforeMount() {
    fetch(`http://api.football-data.org/v2/matches`, {
      method: "GET",
      headers: {
        "X-Auth-Token": `${env.apiKey}`,
      },
    })
      .then((response) => response.json())
      .then((data) => {
        this.todayMatches = data.matches;
      })
      .catch((error) => {
        console.error("Error:", error);
      });
  },
  methods: {
    getHumanDate: function (date) {
      return moment(date).format("DD/MM/YYYY h:mm a");
    },
  },
};
</script>
<style scoped>
/* Hide scrollbar for Chrome, Safari and Opera */
.overflow-x-scroll::-webkit-scrollbar {
  display: none;
}

/* Hide scrollbar for IE, Edge and Firefox */
.overflow-x-scroll {
  -ms-overflow-style: none;  /* IE and Edge */
  scrollbar-width: none;  /* Firefox */
}
</style>