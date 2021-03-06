<template>
  <q-page
    class="flex column"
    :class="bgClass"
  >
    <div class="col q-pt-lg q-mx-md">
      <q-input
        v-model="search"
        @keyup.enter="getWeatherBySearch"
        dark
        placeholder="Search"
        borderless
      >
        <template v-slot:before>
          <q-icon
            @click="getLocation"
            name="my_location"
          ></q-icon>
        </template>

        <template v-slot:hint>Field hint</template>

        <template v-slot:append>
          <q-btn
            round
            dense
            flat
            @click="getWeatherBySearch"
            icon="search"
          ></q-btn>
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          {{ weatherData.name }}
        </div>
        <div class="text-h6 text-weight-light">
          {{ weatherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ Math.round(weatherData.main.temp) }}</span>
          <span class="text-h4 relative-position degree">&deg;C</span>
        </div>
      </div>

      <div class="col text-center">
        <img :src="`https://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`" />
      </div>
    </template>

    <template v-else>
      <div class="col column text-center text-white">
        <div class="col text-h2 text-wieght-thin">Quasar<br>Weather</div>
        <q-btn
          class="col"
          @click="getLocation"
          flat
        >
          <q-icon
            left
            size="3em"
            name="my_location"
          />
          <div>Find my location</div>
        </q-btn>
      </div>
    </template>

    <div class="col skyline"></div>
  </q-page>
</template>

<script lang="ts">
/* eslint-disable @typescript-eslint/no-unsafe-member-access */
/* eslint-disable @typescript-eslint/no-unsafe-call */
/* eslint-disable @typescript-eslint/no-unsafe-assignment */

import { defineComponent } from '@vue/composition-api';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData: null,
      lat: 0,
      lon: 0,
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      apiKey: '25886a546a323c37922608e937d1bd6d',
    };
  },
  computed: {
    bgClass() {
      if (this.weatherData) {
        if ((this.weatherData as any).weather[0].icon.endsWith('n')) {
          return 'bg-night';
        } else {
          return 'bg-day';
        }
      }
    },
  },
  methods: {
    getLocation() {
      this.$q.loading.show();

      if (this.$q.platform.is.electron) {
        void this.$axios.get('https://freegeoip.app/json/').then((response) => {
          this.lat = response.data.latitude;
          this.lon = response.data.longitude;
          this.getWeatherByCoords();
        });
      } else {
        navigator.geolocation.getCurrentPosition((position) => {
          this.lat = position.coords.latitude;
          this.lon = position.coords.longitude;
          this.getWeatherByCoords();
        });
      }
    },
    getWeatherByCoords() {
      this.$q.loading.show();
      void this.$axios
        .get(
          `${this.apiUrl}?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}&units=metric`
        )
        .then((response) => {
          // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
          this.weatherData = response.data;
          this.$q.loading.hide();
        });
    },
    getWeatherBySearch() {
      this.$q.loading.show();
      void this.$axios(
        `${this.apiUrl}?q=${this.search}&appid=${this.apiKey}&units=metric`
      ).then((response) => {
        // eslint-disable-next-line @typescript-eslint/no-unsafe-assignment
        this.weatherData = response.data;
        this.$q.loading.hide();
      });
    },
  },
});
</script>

<style lang="scss">
.q-page {
  background: linear-gradient(to bottom, #136a8a, #267871);

  &.bg-night {
    background: linear-gradient(to bottom, #232526, #414345);
  }

  &.bg-day {
    background: linear-gradient(to bottom, #00b4db, #0083b0);
  }
}

.degree {
  top: -44px;
}

.skyline {
  flex: 0 0 100px;
  background: url('../static/skyline.png');
  background-size: contain;
  background-position: center bottom;
}
</style>