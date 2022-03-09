<template>
  <div class="home">
    <h1>New Report</h1>
    <div>
      Location:
      <input type="text" v-model="newReport.location" />
      Inches:
      <input type="text" v-model="newReport.inches" />
      Time:
      <input type="text" v-model="newReport.time" />
      <button v-on:click="createReport()">Create Report</button>
    </div>
    <h1>All Reports</h1>
    <div v-for="report in reports" v-bind:key="report.id">
      <h2>Zip Code: {{ report.location }}</h2>
      <p>Inches: {{ report.inches }}</p>
      <p>Time: {{ report.time }}</p>
      <button v-on:click="showReport(report)">More info</button>
    </div>
    <dialog id="report-details">
      <form method="dialog">
        <h1>Report info</h1>
        <p>Location: {{ currentReport.location }}</p>
        <p>Inches: {{ currentReport.inches }}</p>
        <p>Time: {{ currentReport.time }}</p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      reports: [],
      newReport: {},
      currentReport: {},
    };
  },
  created: function () {
    this.indexReports();
  },
  methods: {
    indexReports: function () {
      axios.get("/reports").then((response) => {
        console.log("reports index", response);
        this.reports = response.data;
      });
    },
    createReport: function () {
      var params = {
        location: this.newReport.location,
        inches: this.newReport.inches,
        time: this.newReport.time,
      };
      axios
        .post("/reports", params)
        .then((response) => {
          console.log("reports create", response);
          this.reports.push(response.data);
          this.newReport = {};
        })
        .catch((error) => {
          console.log("reports create error", error.response);
        });
    },
    showReport: function (report) {
      this.currentReport = report;
      document.querySelector("#report-details").showModal();
    },
  },
};
</script>
