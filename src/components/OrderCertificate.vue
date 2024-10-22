<template>
  <f-wizard
    v-model="current"
    header-tag="h2"
    @completed="onCompleted"
    disable-initial-focus
  >
    <f-wizard-step
      :key="Step.PERSONUPPGIFTER"
      :use-error-list="false"
      title="Personuppgifter"
    >
      <f-text-field
        v-model="firstName"
        v-validation.required.maxLength="{ maxLength: { length: 100 } }"
      >
        Förnamn
      </f-text-field>
      <f-text-field
        v-model="surname"
        v-validation.required.maxLength="{ maxLength: { length: 100 } }"
      >
        Efternamn
      </f-text-field>
      <f-phone-text-field v-model="phone" v-validation.required>
      </f-phone-text-field>
    </f-wizard-step>

    <f-wizard-step :key="Step.DATUM" :use-error-list="false" title="Datum">
      <f-datepicker-field v-model="date" v-validation.required>
        Från vilket datum ska intyget gälla?
      </f-datepicker-field>
    </f-wizard-step>

    <f-wizard-step
      :key="Step.LEVERANS"
      :use-error-list="false"
      title="Leverans"
    >
      <f-fieldset v-validation.required name="leverans-val">
        <template #label> Hur vill du att vi skickar intyget? </template>
        <template #default>
          <f-radio-field v-model="delivery" value="Hem till mig">
            Hem till mig
          </f-radio-field>
          <f-radio-field v-model="delivery" value="Till Kivra">
            Kivra
          </f-radio-field>
        </template>
      </f-fieldset>
    </f-wizard-step>

    <f-wizard-step
      key="baz"
      :use-error-list="false"
      title="Granska och signera"
    >
      <p>Kolla att allt stämmer innan du signerar.</p>

      <verify-data
        title="1. Personuppgifter"
        button-text="Ändra personuppgifter"
        @change="current = Step.PERSONUPPGIFTER"
      >
        <f-static-field>
          <template #label> Namn </template>
          {{ firstName }} {{ surname }}
        </f-static-field>
      </verify-data>

      <verify-data
        title="2. Datum"
        button-text="Ändra period"
        @change="current = Step.DATUM"
      >
        <f-static-field>
          <template #label> Från vilket datum ska intyget gälla? </template>
          {{ getPeriod() }}
        </f-static-field>
      </verify-data>

      <verify-data
        title="3. Leverans"
        button-text="Ändra leverans"
        @change="current = Step.LEVERANS"
      >
        <f-static-field>
          <template #label> Hur ska vi skicka intyget? </template>
          {{ delivery }}
        </f-static-field>
      </verify-data>

      <f-fieldset v-validation.required name="underskrift">
        <template #label>
          <h2>Underskrift</h2>
        </template>

        <f-checkbox-field value="Ja" v-validation.required>
          Jag intygar på heder och samvete att de uppgifter jag lämnar är
          riktiga och fullständiga.
        </f-checkbox-field>
      </f-fieldset>
      <template #next-button-text>Signera</template>
    </f-wizard-step>
  </f-wizard>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import {
  FCheckboxField,
  FDatepickerField,
  FFieldset,
  FPhoneTextField,
  FRadioField,
  FTextField,
  FWizard,
  FWizardStep,
  FStaticField,
  FPageHeader,
} from "@fkui/vue";

import { DateFormat, FDate } from "@fkui/date";
import VerifyData from "@/components/VerifyData.vue";

export enum Step {
  PERSONUPPGIFTER = "1",
  LEVERANS = "2",
  DATUM = "3",
}

export default defineComponent({
  name: "OrderCertificate",
  components: {
    FCheckboxField,
    FDatepickerField,
    FFieldset,
    FPhoneTextField,
    FRadioField,
    FTextField,
    FWizard,
    FWizardStep,
    FStaticField,
    FPageHeader,
    VerifyData,
  },
  emits: ["completed"],
  data() {
    return {
      current: undefined as undefined | Step,
      firstName: "",
      surname: "",
      phone: "",
      date: "",
      delivery: "",
      Step,
    };
  },
  methods: {
    getPeriod(): string {
      return FDate.fromIso(this.date).toString(DateFormat.LONG);
    },
    onCompleted(): void {
      this.$emit("completed");
    },
  },
});
</script>
