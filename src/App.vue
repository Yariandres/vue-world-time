<template>
  <div
    id="app"
    :class="
      typeof time.datetime != 'undefined' && localtime < '19:00'
        ? 'day'
        : 'night'
    "
  >
    <main>
      <div
        class="headline"
        :class="
          typeof time.datetime != 'undefined' && localtime > '19:00'
            ? 'light'
            : ''
        "
      >
        What is the time, in your region?
      </div>

      <div class="select-box">
        <select v-model="region" class="select-option" required>
          <option disabled value="">Select Region...</option>
          <option>Africa</option>
          <option>America</option>
          <option>Antarctica</option>
          <option>Asia</option>
          <option>Atlantic</option>
          <option>Australia</option>
          <option>Europe</option>
          <option>Indian</option>
          <option>Pacific</option>
        </select>
      </div>

      <div class="search-box">
        <input
          type="text"
          id="search"
          class="search-bar"
          placeholder="Search city..."
          v-model="query"
          @keypress="fetchTime"
        />
      </div>

      <div class="time-box" v-if="typeof time.datetime != 'undefined'">
        <div class="time">
          <p>In - {{ region }}</p>
          <p>{{ query }}</p>
          <p>The time is: {{ localtime }}</p>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      base_url: "http://worldtimeapi.org/api/timezone/",
      region: "",
      query: "",
      time: {},
      localtime: ""
    };
  },

  methods: {
    fetchTime(e) {
      if (e.key === "Enter") {
        fetch(`${this.base_url}${this.region}/${this.query.replace(/ /g, "_")}`)
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
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.day {
  background-image: url("./assets/day-bg.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.night {
  background-image: url("./assets/night-bg.png");
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

.headline {
  text-align: center;
  padding: 30px 0;
  font-size: 20px;
}

.headline.light {
  color: #fff;
}

.select-box {
  margin-bottom: 30px;
  background-color: #fff;
}

.select-option {
  width: 100%;
  padding: 35px;
  font-size: 20px;

  appearance: none;
  outline: none;
  background: none;
}

.search-box {
  margin-bottom: 30px;
  background-color: #fff;
}

.search-bar {
  width: 100%;
  padding: 35px;
  font-size: 20px;

  appearance: none;
  outline: none;
  background: none;
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
