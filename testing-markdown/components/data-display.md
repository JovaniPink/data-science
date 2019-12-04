# Data Display

<div id="app-three"><DataDisplay></DataDisplay></div>

<script>
Vue.component("DataDisplay", {
  props: ["filterlist", "users", "entry"],
  template: `
  <div>
    <div>
        <button v-for="(entry, index) in filterList" :item="entry" :key="index" @click="
          filter = entry;
          active = index;
        " :class="{ active: entry == filter }">
            {{ entry }}
        </button>
    </div>
    <ul class="userWrap">
        <li v-for="(entry, index) in users" v-if="resultsFilter(entry, 'mainLanguage', filter)" :item="entry" :key="index" class="user">
            <h2 class="title">{{ entry.name }}</h2>
            <span class="language">Primary Language: <strong>{{ entry.mainLanguage }}</strong></span>
        </li>
    </ul>
  </div>
  `,
  methods: {
    resultsFilter(entry) {
      if (this.filter !== "All") {
        if (entry[this.fkey] === this.filter) {
          return entry;
        }
      } else {
        return entry;
      }
    }
  }
});

new Vue({
  el: "#app-three",
  data: function() {
    return {
      fkey: "mainLanguage",
      filterList: ["JavaScript", "Python", "PHP", "Java", "All"],
      filter: "All",
      users: []
    };
  },
  created() {
    var apiURL = "https://next.json-generator.com/api/json/get/4JCnNiTCr";
    fetch(apiURL)
      .then(res => res.json())
      .then(res => (this.users = res))
      .catch(error => console.log(error));
  },
  mounted() {
    // console.log(this)
  },
  components: {
    DataDisplay
  }
});

</script>

<style>
    #app-three {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

button {
  background: #74b6cc;
  border: none;
  color: #fff;
  padding: 10px;
  margin: 5px;
}
button.active {
  background: #0089ba;
}
.userWrap {
  list-style-type: none;
  padding: 2%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  flex-direction: row;
}
.user {
  padding: 10px;
  margin: 1% 0;
  border: 1px solid #ddd;
  border-radius: 3px;
  width: 46%;
  text-align: left;
}
h2.title {
  font-size: 1.3rem;
  font-weight: bold;
  margin: 0;
}
.language {
  display: block;
  font-size: 0.9rem;
}

</style>
