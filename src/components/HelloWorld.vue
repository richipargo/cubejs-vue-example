<template>
  <div class="hello">
    <query-builder :cubejs-api="cubejsApi" :query="query">
      <template v-slot:builder="{ measures, setMeasures, availableMeasures }">
        <multiselect
          :multiple="true"
          :customLabel="customLabel"
          @input="(...args) => set(setMeasures, ...args)"
          :value="measures"
          :options="availableMeasures"
          placeholder="Please Select"
          label="Title"
          track-by="name"/>
      </template>
      <template v-slot="{ resultSet }">
        <chart-renderer
          v-if="resultSet"
          :result-set="resultSet" />
      </template>
    </query-builder>
  </div>
</template>

<script>
import cubejs from '@cubejs-client/core';
import Multiselect from 'vue-multiselect';
import { QueryBuilder } from '@cubejs-client/vue';
import ChartRenderer from './ChartRenderer.vue';

const cubejsApi = cubejs(
  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.e30.K9PiJkjegbhnw4Ca5pPlkTmZihoOm42w8bja9Qs2qJg',
  { apiUrl: 'https://react-query-builder.herokuapp.com/cubejs-api/v1' },
);

export default {
  name: 'HelloWorld',
  components: {
    Multiselect,
    QueryBuilder,
    ChartRenderer,
  },
  data() {
    const query = {
      measures: [],
      timeDimensions: [
        {
          dimension: 'LineItems.createdAt',
          granularity: 'month',
        },
      ],
    };

    return {
      cubejsApi,
      selected: undefined,
      query,
    };
  },
  methods: {
    customLabel(a) {
      return a.title;
    },
    set(setMeasures, value) {
      setMeasures(value.map(e => e.name));
    },
  },
};
</script>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>

<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
