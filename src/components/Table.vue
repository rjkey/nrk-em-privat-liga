<template>
  <div class="table-responsive card">
    <table class="table table-hover table-striped" v-if="scores.length">
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
          <td>#{{ score.lesBoisRank }}</td>
          <td>
            <a
              v-bind:href="
                'https://www.nrk.no/sport/manager/bracket-game/view/10/' +
                  score.id
              "
              >{{ score.name }}</a
            >
          </td>
          <td>{{ score.groupScore }}</td>
          <td>{{ score.playoffScore }}</td>
          <td>{{ score.questionScore }}</td>
          <td>{{ score.totalScore }}</td>
          <td>#{{ score.rank }}</td>
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
      players: [11007, 11777, 13645, 14852, 15823, 14008, 11188],
      scores: [],
      lastIndex: 0,
      lastScore: 0,
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
      let tmpValues = [];
      Promise.all(promises).then((values) => {
        values.forEach((value) => {
          tmpValues.push(value.data.bracketEntry);
        });
        tmpValues.sort((a, b) => b.totalScore - a.totalScore);
        this.scores = this.determineLesBoisRank(tmpValues);
      });
    },

    determineLesBoisRank(scores) {
      let lastScore,
        lastIndex = 0;
      scores.forEach((score) => {
        if (score.totalScore !== lastScore) {
          lastScore = score.totalScore;
          lastIndex = lastIndex + 1;
        }
        score.lesBoisRank = lastIndex;
      });
      return scores;
    },
  },
};
</script>

<style scoped>
.card {
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
</style>
