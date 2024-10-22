<template>
  <f-page-header skip-link="main">Demo applikation</f-page-header>
  <main>
    <div class="container">
      <div class="row">
        <div class="col col-sm-12">
          <h1 id="main" tabindex="-1">Beställ intyg</h1>
          <p>Här kan du beställa ett fantastiskt intyg.</p>
          <order-certificate
            v-if="!done"
            @completed="done = true"
          ></order-certificate>
          <f-message-box v-else type="success" layout="short">
            Tack för din beställning!
          </f-message-box>
        </div>
      </div>
    </div>
  </main>
</template>

<script lant="ts">
import { defineComponent } from "vue";
import OrderCertificate from "./components/OrderCertificate.vue";
import { FPageHeader, FMessageBox } from "@fkui/vue";

export default defineComponent({
  components: {
    OrderCertificate,
    FPageHeader,
    FMessageBox,
  },
  data() {
    return {
      done: false,
    };
  },
  methods: {
    onCompleted() {
      this.done = true;
    },
  },
});
</script>
