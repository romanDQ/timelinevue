<template>
  <v-timeline>
    <v-timeline-item
      v-for="person in Data"
      :key="person._id"
      medium
    >
      <template v-slot:opposite>
        <span
          :class="`font-weight-bold black--text`"
          v-text="person.registered"
        ></span>
      </template>
      <v-card class="elevation-2">
        <h1
        :class="`headline font-weight-bold black--text`"
        v-text="person.name.first + ` ` + person.name.last"
        />
        <v-card-title class="headline">About</v-card-title>
        <div v-text="person.about" />
        <v-card-text>
          <div v-for="(tag, i) in person.tags" :key="i">
          <div v-text= "`#` + tag" />
        </div>
        </v-card-text>
      </v-card>
    </v-timeline-item>
  </v-timeline>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  name: "timeline",
  data() {
    return {
      baseUrl: process.env.VUE_APP_BASE_URL,
      Data: null
    };
  },
  created() {
    this.getData();
  },
  methods: {
    getData() {
      axios.get(this.baseUrl + "timeline.json").then(response => {
        this.sortData(response.data);
      });
    },
     sortData(data) {
       this.Data = data.sort(
         (a, b) =>
           moment(a.registered).format("YYYYMMDD") - moment(b.registered).format("YYYYMMDD")
       );
     }
  }
};
</script>
