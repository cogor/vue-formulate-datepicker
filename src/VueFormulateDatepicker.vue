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
  computed: {
    date: {
      get() {
        if (this.context.model) {
          return this.formatDate(this.context.model);
        } else return null;
      },
      set(val) {
        this.context.rootEmit("input", this.formatDate(val));
        this.context.model = this.formatDate(val);
      },
    },
  },
  watch: {
    "context.model": {
      handler(val) {
        this.context.model = this.formatDate(val);
        this.context.rootEmit("input", this.formatDate(val));
      },
    },
    "context.value": {
      handler(val) {
        this.context.model = this.formatDate(val);
        this.context.rootEmit("input", this.formatDate(val));
      },
    },
  },
  methods: {
    formatDate(date) {
      if (!date) return null;
      let d = new Date(date);
      let year = new Intl.DateTimeFormat("en", { year: "numeric" }).format(d);
      let month = new Intl.DateTimeFormat("en", { month: "2-digit" }).format(d);
      let day = new Intl.DateTimeFormat("en", { day: "2-digit" }).format(d);
      return `${year}-${month}-${day}`;
    },
  },
  components: { Datepicker },
};
</script>
