<template>
  <h1>Cards list</h1>
  <CardsList>
    <template #card="{ card, toggleLike, id, time }">
      <div class="card">
        <p class="title">{{ card.title }}</p>
        <p>{{ card.desc }}</p>
        <p class="time">{{ time }}</p>
        <img src="../assets/edit-246-32.png" @click="editCard(id)" />
        <img
          v-if="card.liked"
          src="../assets/like.png"
          width="30"
          height="30"
          @click="toggleLike(card)"
        />
        <img
          v-else
          src="../assets/unlike.png"
          width="32"
          height="32"
          @click="toggleLike(card)"
        />
      </div>
    </template>
  </CardsList>
</template>

<script>
import CardsList from "../components/Cards-list.vue";
export default {
  name: "App",
  components: {
    CardsList,
  },
  methods: {
    editCard(id) {
      this.$router.push({ name: "edit", params: { id: id } });
    },
  },
};
</script>
<style scoped>
.title {
}
.time {
  font-style: italic;
}
</style>
