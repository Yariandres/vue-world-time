<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          id="search"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchTime"
        />
      </div>
      <div class="time-wrap">
        <div class="location-box">
          <div class="location">{{ time.timezone }}</div>
        </div>
      </div>

      <div class="time-box">
        <div class="time">The time is: {{ localtime }}</div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      base_url: "http://worldtimeapi.org/api/timezone/Europe/",
      query: "",
      time: {},
      localtime: ""
    };
  },

  methods: {
    fetchTime(e) {
      if (e.key === "Enter") {
        fetch(`${this.base_url}${this.query}`)
          .then(res => {
            return res.json();
          })
          .then(this.setResults);
      }
    },

    setResults(results) {
      this.time = results;
      this.localtime = results.datetime.substring(11, 19);
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#app {
  /* background-image: url("./assets/day-bg.png"); */
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 35px;
  color: #313131;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(255, 255, 255, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}
.location-box .location {
  color: #0e1335;
  font-size: 42px;
  font-weight: 500px;
  text-align: center;
  text-shadow: 1px 3px rgba(255, 255, 255, 0.25);
}

.location-box .date {
  color: #0e1335;
  font-size: 30px;
  font-weight: 300px;
  text-align: center;
  font-style: italic;
  text-shadow: 1px 3px rgba(255, 255, 255, 0.25);
}

.time-box {
  text-align: center;
  text-shadow: 1px 3px rgba(255, 255, 255, 0.25);
}

.time-box .time {
  padding: 10px 25px;
  color: #0e1335;
  font-size: 70px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.55);
  border-radius: 16px;
  margin: 30px 0px;
}
</style>
