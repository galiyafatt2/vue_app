<template>
  <div class="new">
    <h1>New card</h1>
  </div>
  <div class="info">
    <p><input v-model="card.title" placeholder="add title" /></p>
    <p><textarea v-model="card.desc" placeholder="add description" /></p>
    <button @click="add(card.title, card.desc)">Save</button>
  </div>
</template>

<script>
export default {
  name: "Edit-view",
  data: function () {
    return {
      card: { title: "", desc: "", time: Date, liked: false },
      cards: [{ title: "", desc: "", time: Date, liked: false }],
      save_button: false,
    };
  },
  mounted() {
    if (localStorage.getItem("cards")) {
      try {
        this.cards = JSON.parse(localStorage.getItem("cards"));
      } catch (e) {
        localStorage.removeItem("cards");
      }
    }
  },
  methods: {
    add(n, d) {
      if (!n || !d || n.length < 5 || n.length > 254) {
        alert("fields can't be empty or too short/long");
        return;
      }
      this.cards.push({
        title: n,
        desc: d,
        time: new Date(),
        liked: false,
      });
      this.saveCards();
      this.save_button = true;
      this.$router.push("/");
    },
    saveCards() {
      const parsed = JSON.stringify(this.cards);
      localStorage.setItem("cards", parsed);
    },
  },
  beforeRouteLeave(to, from) {
    if (this.save_button) return true;
    to = from;
    const answer = window.confirm(
      "Do you really want to leave? you have unsaved changes!"
    );
    if (!answer) return false;
  },
};
</script>

<style scoped>
.info {
  width: 15%;
  margin-left: 41%;
  border: 1px solid black;
  padding: 20px;
  background-color: lightgray;
}
</style>
