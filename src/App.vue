<template>
  <div class="container" :class="{ cold: cold }">
    <div class="main">
      <h1 class="title">Weather App</h1>
      <form class="searchForm" @submit.prevent="submitForm">
        <div class="inputContainer">
          <input
            type="text"
            v-model="search"
            id="searchInput"
            placeholder="Enter a City , Country "
          />
        </div>
        <button class="submitBtn">Search</button>
      </form>
      <div class="results" v-if="results">
        <p class="place">{{ results.name }},{{ results.sys.country }}</p>
        <p class="date">
          {{ new Date().toLocaleDateString(undefined, options) }}
        </p>
        <div class="temp">
          <p>
            {{ temp }}
            <sup class="sup">o</sup>
            F
          </p>
        </div>
        <p class="current">{{ results.weather[0].description }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line prettier/prettier
  name: "App",
  data() {
    return {
      search: "",
      results: "",
      api_key: "250fcac1cce732058b63a4bcb770870f",
      temp: 0,
      cold: false,
      options: {
        weekday: "long",
        year: "numeric",
        month: "long",
        day: "numeric"
      }
    };
  },
  methods: {
    submitForm() {
      (async () => {
        fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.search}&appid=${this.api_key}`
        )
          .then(res => res.json())
          .then(parsed => {
            this.search = "";
            this.results = parsed;
            const curTemp = Math.round(
              ((parseFloat(parsed.main.temp) - 273.15) * 9) / 5 + 32
            );
            this.temp = curTemp;
            this.cold = curTemp < 50 ? true : false;
          });
      })();
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@100;200;300;400;500;600;700;800;900&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: inherit;
}

html {
  font-size: 62.5%;
  font-family: "Montserrat", sans-serif;
  color: black;
  height: 100vh;
  width: 100vw;
}

.container {
  width: 100vw;
  height: 100vh;
  background: url("./assets/sun.jpg");
  background-position: center;
  background-size: cover;
  transition: 3s;
  display: flex;
  align-items: center;
  justify-content: center;

  &.cold {
    background: url("./assets/winter.jpg");
  }
}

.main {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-direction: column;

  padding: 4rem 10rem;
  width: 100vw;
  height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.1),
    rgba(0, 0, 0, 0.8)
  );
}

.title {
  text-align: center;
  font-size: 4rem;
  font-weight: 800;
  text-transform: uppercase;
  margin-bottom: 3rem;
}

.searchForm {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  & .inputContainer {
    border-radius: 0.5rem;
    border: 2px solid transparent;

    &:focus-within {
      border: 2px solid black;
    }
    & #searchInput {
      padding: 1rem;
      font-size: 2.2rem;
      border-radius: 0.5rem;

      &,
      &:focus,
      &:active,
      &:hover {
        border: 0;
        outline: none;
      }
    }
  }

  & .submitBtn {
    font-size: 1.4rem;
    font-weight: 500;
    &,
    &:focus,
    &:active,
    &:hover {
      border: 0;
      outline: none;
    }
    margin-top: 2rem;
    border-radius: 0.5rem;
    text-transform: uppercase;
    padding: 1rem 2rem;
    background-color: white;
    &:hover {
      opacity: 0.7;
    }
  }
}

.results {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  margin-top: 4rem;
  font-size: 1.8rem;
  font-weight: 600;
  color: white;
  & * {
    margin-top: 1.5rem;
  }

  .place {
    font-style: italic;
    text-transform: uppercase;
  }

  .temp {
    display: flex;
    justify-content: center;
    padding: 5rem;
    font-size: 6rem;
    background: rgba(white, 0.8);
    color: black;
    border-radius: 1rem;
  }

  .current {
    font-size: 3rem;
    font-weight: 800;
    text-transform: uppercase;
  }
}
</style>
