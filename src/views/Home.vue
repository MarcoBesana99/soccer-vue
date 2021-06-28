<template>
  <div>home</div>
  <div>
    <div v-for="match in todayMatches" :key="match.id">
      {{ match.competition.name }}
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import NavBar from '@/components/NavBar.vue'
import SideBar from '@/components/SideBar.vue'
import MatchItem from '@/components/MatchItem.vue'
import env from "@/env.js";
export default {
  name: "Home",
  data () {
    return {
      todayMatches : []
    }
  },
  components : {
    NavBar, SideBar, MatchItem
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
        this.todayMatches = data.matches
        console.log("Success:", data);
      })
      .catch((error) => {
        console.error("Error:", error);
      });
  },
};
</script>
