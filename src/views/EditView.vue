<template>
  <div class="edit">
    <h1>Card info</h1>
  </div>
  <div class="info">
    <h2>Title</h2>
    <p>
      <input v-model="card.title" placeholder="add title" />
    </p>
    <h2>Description</h2>
    <p>
      <textarea v-model="card.desc" placeholder="add description" />
    </p>
    <button @click="edit(cards.indexOf(card), card.title, card.desc)">
      Save
    </button>
    <button @click="removeCard(this.$route.params.id)">Delete</button>
  </div>
</template>

<script>
export default {
  name: "Edit-view",
  data: function () {
    return {
      card: "",
      cards: [{ title: "", desc: "", time: Date, liked: false }],
      edit_button: false,
    };
  },
  mounted() {
    if (localStorage.getItem("cards")) {
      try {
        this.cards = JSON.parse(localStorage.getItem("cards"));
      } catch (e) {
        localStorage.removeItem("cards");
      }
      this.card = this.cards[this.$route.params.id];
    }
  },
  beforeRouteLeave(to, from) {
    to = from;
    if (this.edit_button) return true;
    const answer = window.confirm(
      "Do you really want to leave? you have unsaved changes!"
    );
    if (!answer) return false;
  },
  methods: {
    edit(id, n, d) {
      if (!n || !d || n.length < 5 || n.length > 254) {
        alert("fields can't be empty or too short/long");
        return;
      }
      this.cards[id] = {
        title: n,
        desc: d,
        liked: false,
        time: this.cards[id].time,
      };
      this.saveCards();
      this.edit_button = true;
      this.$router.push("/");
    },
    saveCards() {
      const parsed = JSON.stringify(this.cards);
      localStorage.setItem("cards", parsed);
    },
    removeCard(id) {
      this.edit_button = true;
      this.cards.splice(id, 1);
      this.saveCards();
      this.$router.push("/");
    },
  },
};
</script>

<style scoped>
.info {
  width: 15%;
  margin-left: 42%;
  border: 1px solid black;
  padding: 20px;
  background-color: lightgray;
}
</style>
