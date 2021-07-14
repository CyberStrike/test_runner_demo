<template>
  <div class="container">
    <div class="row align-items-center justify-content-center">
      <div class="col col-4">
        <div class="d-flex justify-content-between">
          <h3>Tests</h3>
          <button
            class="btn btn-sm btn-primary rounded-pill px-3"
            @click="concurrentTestRunner"
          >
            Run All
          </button>
        </div>
        <ul class="list-inline">
          <li class="list-inline-item">
            <strong>Total</strong>: {{ tests.length }}
          </li>
          <li class="list-inline-item">
            <strong>Running:</strong> {{ totalRunning }}
          </li>
          <li class="list-inline-item">
            <strong> Passed: </strong> {{ totalPassed }}
          </li>
          <li class="list-inline-item">
            <strong> Fail: </strong> {{ totalFail }}
          </li>
        </ul>
        <div class="row">
          <TestReportList class="mx-auto mt-4" :tests="tests" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TestReportList from "@/components/TestReportList";

const IDLE = 0;
const RUNNING = 1;
const PASSED = 2;
const FAILED = 3;

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
  computed: {
    totalRunning() {
      return this.statusTotals(RUNNING);
    },
    totalPassed() {
      return this.statusTotals(PASSED);
    },
    totalFail() {
      return this.statusTotals(FAILED);
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
    statusTotals(status) {
      return this.tests.filter((test) => status === test.status).length;
    },
  },
};
</script>
