<template>
  <div>
    <h1>Generate a character name</h1>
    <div>
      <p>
        Female names are represented with
        <span style="color: lightpink">light pink</span> as a background while male names are represented with
        <span
          style="color: lightblue"
        >light blue</span> as a background.
      </p>
    </div>
    <UserSearch v-on:userSubmit="searchUser" style="padding: 20px;"/>
    <div v-for="(item, i) in nameArray" :key="i">
      <transition name="highlight-clock" appear>
        <NameCard v-bind:gender="item.gender">
          <h2 style="text-align: left;">
            {{ item.name }} {{ item.surname }}
            <br>
            <span style="font-size: 14px;">Origin: {{ item.region }}</span>
          </h2>

          <v-spacer></v-spacer>
          <v-btn color="info" @click="addName(item.name, item.surname)">Select</v-btn>
          <br>
        </NameCard>
      </transition>
    </div>
  </div>
</template>

<script>
import UserSearch from "@/components/UserSearch.vue";
import NameCard from "@/components/NameCard.vue";
import axios from "axios";

export default {
  name: "search",
  components: {
    UserSearch,
    NameCard
  },
  data: () => {
    return {
      queryString: "?amount=",
      queryAmount: "1",
      nameArray: [],
      firstNombre: "",
      lastNomber: ""
    };
  },
  methods: {
    searchUser(maxNum) {
      this.queryAmount = maxNum;
      this.nameArray = [];
      axios
        .get(
          `https://uinames.com/api/${this.queryString}${this.queryAmount}`,
          {}
        )
        .then(response => {
          this.nameArray = response.data;
          console.log(response.data);
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    addName(name, surname) {
      this.firstNombre = name;
      this.lastNomber = surname;
      this.$store.state.firstName = name;
      this.$store.state.lastName = surname;
      this.showName = true;
      this.$router.push({ name: "home" });
    }
  }
};
</script>

<style scoped>
.highlight-clock-enter {
  opacity: 0;
}
.highlight-clock-enter-active {
  transition: opacity 3s;
  animation: move-in 1s ease-out forwards;
}
@keyframes move-in {
  from {
    transform: translateX(-100%);
  }
  to {
    transform: translateX(0);
  }
}
</style>
