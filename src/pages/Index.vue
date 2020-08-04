<template>
  <q-page class="flex column">
    <div class="col q-pt-lg q-mx-md">
      <q-input v-model="text" :dense="dense" label="Label" counter bottom-slots maxlength="12">
        <template v-slot:before>
          <q-icon name="event"></q-icon>
        </template>

        <template v-slot:hint>Field hint</template>

        <template v-slot:append>
          <q-btn round dense flat icon="add"></q-btn>
        </template>
      </q-input>
    </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api';

export default defineComponent({
  name: 'PageIndex',
});
</script>

<style lang="scss">
.q-page {
  background: linear-gradient(to bottom, #136a8a, #267871);
}
</style>