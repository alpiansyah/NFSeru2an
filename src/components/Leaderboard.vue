<template>
  <div class="leaderboard">
    <transition-group
      move-class="leaderboard__item--move"
      v-if="users.length > 0"
    >
      <LeaderboardItem
        v-for="user in sortedUsers"
        :key="user.id"
        :user="user"
        @step="handleStep"
      />
    </transition-group>
    <p class="message" v-else>Nothing to show</p>
  </div>
</template>

<script>
import LeaderboardItem from "./LeaderboardItem.vue";

export default {
  name: "Leaderboard",
  components: {
    LeaderboardItem: LeaderboardItem,
  },
  data() {
    return {
      users: [],
    };
  },
  mounted() {
    fetch("../users.json")
      .then((res) => res.json())
      .then((res) => (this.users = res.users));
  },
  methods: {
    handleStep(event) {
      const { userId, step } = event;
      let user = this.users.find((user) => user.id === userId);
      user.score += step;
      user.score = user.score < 0 ? 0 : user.score;
      this.users = [...this.users];
    },
  },
  computed: {
    sortedUsers() {
      return [...this.users].sort((a, b) =>
        a.score === b.score
          ? a.name.localeCompare(b.name)
          : a.score > b.score
          ? -1
          : 1
      );
    },
  },
};
</script>

<style>
.leaderboard {
  border: 1px solid #bbb;
  border-radius: 0.5rem;
  box-shadow: 1px 1px 10px #777;
  font-family: Arial, Helvetica, sans-serif;
  list-style: none;
  margin: 5rem auto 0;
  padding: 2rem;
  width: 20rem;
}
.leaderboard__item--move {
  transition: transform 0.2s;
}
.message {
  text-align: center;
}
</style>