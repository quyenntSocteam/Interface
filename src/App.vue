<template>
  <div id="app" class="container">
    <div class="row">
      <AddAppointments @add="addItem" />
      <SearchAppointments @searchRecords="returnSearch" />
      <AppointmentsList
        :appointments="searchApts ? searchApts : appointments"
        @edit="editItem"
        @remove="removeItem"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import _ from "lodash";

import AppointmentsList from "./components/AppointmentsList.vue";
import AddAppointments from "./components/AddAppointments.vue";
import SearchAppointments from "./components/SearchAppointments.vue";

export default {
  name: "App",
  data() {
    return {
      title: "App Interface",
      appointments: [],
      uuidIndex: 0,
      searchTerms: "",
    };
  },
  components: {
    AppointmentsList,
    AddAppointments,
    SearchAppointments,
  },
  computed: {
    searchApts() {
      return this.appointments.filter((item) => {
        return (
          item.petName.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.petOwner.toLowerCase().match(this.searchTerms.toLowerCase()) ||
          item.aptNotes.toLowerCase().match(this.searchTerms.toLowerCase())
        );
      });
    },
  },
  methods: {
    removeItem(item) {
      this.appointments = _.without(this.appointments, item);
    },
    editItem(uuid, itemName, itemEdited) {
      const uuidIndex = _.findIndex(this.appointments, {
        uuid,
      });
      console.log(uuidIndex);
      this.appointments[uuidIndex][itemName] = itemEdited;
    },
    addItem(newItem) {
      newItem.uuid = this.uuidIndex;
      this.uuidIndex++;
      this.appointments.push(newItem);
      this.sortItem();
    },
    sortItem() {
      var dataSort = this.appointments.sort(function(a, b) {
        return new Date(b.aptDate) - new Date(a.aptDate);
      });
      this.appointments = dataSort;
    },
    returnSearch(term) {
      this.searchTerms = term;
    },
  },
  mounted() {
    axios.get("./data/appointments.json").then(
      (response) =>
        (this.appointments = response.data.map((item) => {
          item.uuid = this.uuidIndex;
          this.uuidIndex++;
          return item;
        }))
    );
  },
};
</script>

<style></style>
