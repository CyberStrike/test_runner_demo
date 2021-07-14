<template>
  <div class="container">
    <TestReportList :tests="sortedTests" />
    <button class="btn btn-default" @click="concurrentTestRunner">
      Run All
    </button>
  </div>
</template>

<script>
import TestReportList from "@/components/TestReportList";

// const IDLE = 0;
const RUNNING = 1;
const PASSED = 2;
const FAILED = 3;

export default {
  name: "TestRunnerPage",
  components: { TestReportList },
  data: () => ({
    tests: [
      { status: null, id: 1, name: "Test" },
      { status: null, id: 2, name: "Test A Thing" },
      { status: null, id: 3, name: "Test Another Thing" },
      { status: null, id: 4, name: "Test One More Thing" },
    ],
  }),
  computed: {
    sortedTests() {
      return this.tests;
    },
  },
  methods: {
    async concurrentTestRunner() {
      await Promise.all(this.tests.map((test) => this.runTest(test)));
    },
    updateTestStatus(status, test) {
      test.status = status;
      const testIndex = this.tests.findIndex((t) => test.id === t.id);
      this.tests.splice(testIndex, 1, test);
    },
    testCase(update) {
      return [Promise.resolve(update(PASSED)), Promise.resolve(update(FAILED))][
        this.randomInt(0, 1)
      ];
    },
    runTest(test) {
      const MINIMUM_TIME = 1000;
      const MAXIMUM_TIME = 5000;

      this.updateTestStatus(RUNNING, test);

      setTimeout(
        () =>
          this.testCase((status) => {
            this.updateTestStatus(status, test);
          }),
        this.randomInt(MINIMUM_TIME, MAXIMUM_TIME)
      );
    },
    randomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
  },
};
</script>
