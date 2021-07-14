<template>
  <div class="card" style="width: 18rem">
    <ul class="list-group list-group-flush">
      <li
        class="list-group-item"
        v-for="(test, index) in tests"
        :id="index"
        :key="index"
      >
        {{ statusToText(test.status) }} > {{ test.name }}
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
  methods: {
    statusToText(status) {
      return ["idle", "running", "failed", "passed"][status];
    },
  },
};
</script>
