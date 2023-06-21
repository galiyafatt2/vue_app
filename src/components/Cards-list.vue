<template>
  <div>
    <img
      alt="date by desc"
      v-if="this.sort_date === true"
      src="../assets/arrow-23-32.png"
      @click="sortDate()"
    />
    <img
      alt="date by asc"
      v-else
      src="../assets/up-arrow-10-32.png"
      @click="sortDate()"
    />
    <img
      alt="layout to table"
      v-if="this.list_layout === false"
      src="../assets/table-11-32.png"
      @click="changeLayout()"
    />
    <img
      alt="layout to list"
      v-else
      src="../assets/list-26-32.png"
      @click="changeLayout()"
    />
    <div v-if="this.list_layout === false" class="grid">
      <div class="create" @click="createCard()">
        <img alt="add new card" src="../assets/plus-60-128.png" />
      </div>
      <fragment v-for="card in cards_copy" v-bind:key="card">
        <slot
          name="card"
          :card="card"
          :toggleLike="toggleLike"
          :id="cards.indexOf(card)"
          :time="putDate(card.time)"
        />
      </fragment>
    </div>
    <div v-else class="list">
      <div class="create" @click="createCard()">
        <img src="../assets/plus-60-128.png" />
      </div>
      <fragment v-for="card in cards_copy" v-bind:key="card">
        <slot
          name="card"
          :card="card"
          :toggleLike="toggleLike"
          :id="cards.indexOf(card)"
          :time="putDate(card.time)"
        />
      </fragment>
    </div>
  </div>
</template>

<script>
export default {
  name: "Cards-list",
  data: function () {
    return {
      cards: [{ title: "1", desc: "111", time: Date, liked: false }],
      cards_copy: Array,
      sort_date: false,
      list_layout: false,
    };
  },
  mounted() {
    if (localStorage.getItem("cards")) {
      try {
        this.cards = JSON.parse(localStorage.getItem("cards"));
      } catch (e) {
        localStorage.removeItem("cards");
      }
      if (this.sort_date === false) this.cards_copy = this.cards.slice();
    }
  },
  methods: {
    saveCards() {
      const parsed = JSON.stringify(this.cards);
      localStorage.setItem("cards", parsed);
    },
    toggleLike(card) {
      card.liked = !card.liked;
      this.saveCards();
    },
    createCard() {
      this.$router.push("/create");
    },
    putDate(date) {
      let now = new Date();
      let then = new Date(Date.parse(date));
      let gap = now - then;
      let minute = 60000;
      let hour = minute * 60;
      let day = hour * 24;
      let week = day * 7;
      switch (true) {
        case gap < minute:
          return "just now";
        case gap < hour:
          return `${Math.floor(gap / minute)} minutes ago`;
        case gap < day:
          return `${Math.floor(gap / hour)} hours ago`;
        case gap < week:
          return `${Math.floor(gap / day)} days ago`;
        default:
          return `at ${then.getDate()}-${then.getMonth()}-${then.getFullYear()}`;
      }
    },
    sortDate() {
      this.sort_date = !this.sort_date;
      this.cards_copy.reverse();
    },
    changeLayout() {
      this.list_layout = !this.list_layout;
    },
  },
};
</script>

<style scoped>
fragment {
  border: 1px solid black;
  background-color: aliceblue;
}
.grid {
  width: 60%;
  margin-left: 23%;
  display: grid;
  grid-template-columns: repeat(3, 30%);
  grid-gap: 10px;
  overflow-x: hidden;
}
.list {
  width: 60%;
  margin-left: 20%;
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  gap: 10px;
}
.create {
  position: relative;
  border: 1px dashed black;
  grid-column: 1;
  grid-row: 1;
  background-color: gainsboro;
}
</style>
