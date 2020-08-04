<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-mx-md">
      <q-input
        v-model="search"
        dark
        placeholder="Search"
        borderless
      >
        <template v-slot:before>
          <q-icon name="my_location"></q-icon>
        </template>

        <template v-slot:hint>Field hint</template>

        <template v-slot:append>
          <q-btn
            round
            dense
            flat
            icon="search"
          ></q-btn>
        </template>
      </q-input>
    </div>

    <div class="col text-white text-center">
      <div class="text-h4 text-weight-light">
        Manchester
      </div>
      <div class="text-h6 text-weight-light">
        Rain
      </div>
      <div class="text-h1 text-weight-thin q-my-lg relative-position">
        <span>8</span>
        <span class="text-h4 relative-position degree">&deg;</span>
      </div>
    </div>

    <div class="col text-center">
      <img
        src="https://www.fillmurray.com/100/100"
        alt="Bill"
      >
    </div>

    <div class="col skyline"></div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      search: '',
    };
  },
});
</script>

<style lang="scss">
.q-page {
  background: linear-gradient(to bottom, #136a8a, #267871);
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