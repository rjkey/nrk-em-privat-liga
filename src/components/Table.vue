<template>
  <div class="table-responsive card">
    <table class="table table-hover table-striped">
      <thead>
        <tr>
          <th scope="col">Les bois rank</th>
          <th scope="col">Team name</th>
          <th scope="col">Group score</th>
          <th scope="col">Playoff score</th>
          <th scope="col">Question score</th>
          <th scope="col">Total score</th>
          <th scope="col">National rank</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(score, index) in scores" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ score.name }}</td>
          <td>{{ score.groupScore }}</td>
          <td>{{ score.playoffScore }}</td>
          <td>{{ score.questionScore }}</td>
          <td>{{ score.totalScore }}</td>
          <td>{{ score.rank }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Table",
  props: {
    msg: String,
  },
  data() {
    return {
      players: [11007],
      scores: [],
    };
  },
  created() {
    this.fetchUserScores();
  },
  methods: {
    async fetchUserScores() {
      let promises = [];
      this.players.forEach((player) => {
        const promise = new Promise((resolve, reject) => {
          axios
            .get(`https://bracket.api.scoutgg.net/bracket_entries/${player}`)
            .then(
              (response) => {
                resolve(response);
              },
              (error) => reject(error)
            );
        });
        promises.push(promise);
      });
      Promise.all(promises).then((values) => {
        values.forEach((value) => {
          this.scores.push(value.data.bracketEntry);
        });
      });
      this.scores.sort((a, b) => (a.totalScore > b.totalScore ? 1 : -1));
    },
  },
};
</script>

<style scoped>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>
