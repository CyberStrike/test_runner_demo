<template>
  <div class="card p-0" style="width: 18rem">
    <ul class="list-group list-group-flush">
      <li
        :class="['list-group-item', statusClass(test.status)]"
        v-for="(test, index) in sortedTests"
        :id="index"
        :key="index"
      >
        {{ test.name }}: {{ statusToText(test.status) }}
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: "TestReportList",
  props: {
    tests: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    sortedTests() {
      return [...this.tests].sort((a, b) => a.status - b.status);
    },
  },
  methods: {
    statusToText(status) {
      return ["idle", "running", "passed", "failed"][status];
    },
    statusClass(status) {
      return {
        idle: "",
        failed: "bg-danger text-white",
        passed: "bg-success text-white",
        running: "bg-warning",
      }[this.statusToText(status)];
    },
  },
};
</script>
