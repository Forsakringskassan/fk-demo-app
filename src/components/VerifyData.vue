<template>
  <div class="granska-steg">
    <h3 v-if="title" class="spacing-bottom">{{ title }}</h3>
    <slot></slot>
    <button
      type="button"
      class="button button--tertiary button--medium button--tertiary--underline button--align-text"
      @click="goToStep"
    >
      {{ buttonText }}
    </button>
    <div class="ruler"></div>
  </div>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import { FIcon } from "@fkui/vue";

export default defineComponent({
  name: "VerifyData",
  components: { FIcon },
  props: {
    title: {
      type: String,
      required: true,
    },
    buttonText: {
      type: String,
      required: true,
    },
  },
  emits: ["change"],
  methods: {
    goToStep() {
      this.$emit("change");
    },
  },
});
</script>
