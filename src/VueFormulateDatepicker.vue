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
import * as lang from "@sum.cumo/vue-datepicker/dist/locale/index.esm";
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
      language: lang[this.context.attributes.language],
    };
  },
  watch: {
    date() {
      this.context.model = this.formatDate(this.date);
    },
  },
  computed: {
    date: {
      get() {
        return new Date(this.context.model);
      },
      set(val) {
        this.context.model = this.formatDate(val);
      },
    },
  },
  mounted() {
    this.context.model = this.formatDate(this.context.model);
  },
  methods: {
    formatDate(date) {
      let d = date;
      let year = new Intl.DateTimeFormat("en", { year: "numeric" }).format(d);
      let month = new Intl.DateTimeFormat("en", { month: "2-digit" }).format(d);
      let day = new Intl.DateTimeFormat("en", { day: "2-digit" }).format(d);
      return `${year}-${month}-${day}`;
    },
  },
  components: { Datepicker },
};
</script>
