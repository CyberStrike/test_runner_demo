<template>
  <div class="container">
    <TestReportList :tests="tests" />
    <button class="btn btn-primary" @click="concurrentTestRunner">
      Run All
    </button>
  </div>
</template>

<script>
import TestReportList from "@/components/TestReportList";

const IDLE = 0;
const RUNNING = 1;
const PASSED = 2;
const FAILED = 3;

const testStatus = {
  IDLE: 0,
  RUNNING: 1,
  PASSED: 2,
  FAILED: 3,
};

export default {
  name: "TestRunnerPage",
  components: { TestReportList },
  data: () => ({
    tests: [
      { status: IDLE, id: 1, name: "Test" },
      { status: IDLE, id: 2, name: "Test A Thing" },
      { status: IDLE, id: 3, name: "Test Another Thing" },
      { status: IDLE, id: 4, name: "Test One More Thing" },
    ],
  }),
  methods: {
    async concurrentTestRunner() {
      await Promise.all(this.tests.map((test) => this.runTest(test)));
    },
    updateTestStatus(status, test) {
      test.status = status;
      const testIndex = this.tests.findIndex((t) => test.id === t.id);
      console.debug(test.id, Object.keys(testStatus)[test.status]);
      this.tests.splice(testIndex, 1, test);
      return test;
    },
    runTest(test) {
      const MINIMUM_TIME = 1000;
      const MAXIMUM_TIME = 5000;

      this.updateTestStatus(RUNNING, test);
      const result = [PASSED, FAILED][this.randomInt(0, 1)];

      return new Promise((resolve) => {
        setTimeout(() => {
          this.updateTestStatus(result, test);
          resolve(result);
        }, this.randomInt(MINIMUM_TIME, MAXIMUM_TIME));
      });
    },
    randomInt(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
  },
};
</script>
