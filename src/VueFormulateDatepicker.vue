<template>
  <Datepicker
    :class="`formulate-input-element formulate-input-element--${context.type}`"
    :input-class="context.attributes.class"
    :data-type="context.type"
    v-model="date"
    :options="context.options"
    v-bind="context.attributes"
    :language="language"
    @blur="context.blurHandler"
  />
</template>

<script>
import Datepicker from "@sum.cumo/vue-datepicker";

export default {
  name: "VueFormulateDatapicker",
  props: {
    context: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      language: {},
      date: null,
    };
  },
  watch: {
    date() {
      let d = this.date;
      let year = new Intl.DateTimeFormat("en", { year: "numeric" }).format(d);
      let month = new Intl.DateTimeFormat("en", { month: "2-digit" }).format(d);
      let day = new Intl.DateTimeFormat("en", { day: "2-digit" }).format(d);
      this.context.model = `${year}-${month}-${day}`;
    },
  },
  methods: {
    getLanguage() {
      let lang = this.context.attributes.language ?? "en";
      import(
        `@sum.cumo/vue-datepicker/dist/locale/translations/${lang}.js`
      ).then((result) => {
        this.language = result.default;
      });
    },
  },
  mounted() {
    this.getLanguage();
  },
  components: { Datepicker },
};
</script>
