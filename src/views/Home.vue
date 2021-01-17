<template>
  <div class="container mt-5">
    <div class="mb-3">
      <label class="form-label">Search</label>
      <input
        v-model="search"
        type="text"
        class="form-control"
        placeholder="Search from table"
      />
    </div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Field Data</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(data, index) in resultQuery" :key="index">
          <th scope="row">{{ data.index }}</th>
          <td>{{ data.title }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data: () => ({
    dataSet: {},
    search: ""
  }),
  computed: {
    resultQuery() {
      if (this.search) {
        return this.dataSet.filter(item => {
          return this.search
            .toLowerCase()
            .split(" ")
            .every(v => item.title.toLowerCase().includes(v));
        });
      } else {
        return this.dataSet;
      }
    },
  },
  async mounted() {
    await this.init();
  },
  methods: {
    async init() {
      await fetch("https://api.publicapis.org/categories")
        .then(res => {
          return res.json();
        })
        .then(response => {
          this.dataSet = response.map((res, index) => {
            return { index: index + 1, title: res };
          });
        });
    }
  }
};
</script>

<style lang="scss" scoped></style>
