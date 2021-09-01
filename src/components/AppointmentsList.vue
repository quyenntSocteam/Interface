<template>
  <div class="col-12 col-md-10 col-lg-7">
    <div class="list-group list-group-flush">
      <div
        class="list-group-item d-flex align-items-start"
        v-for="item in appointments"
        :key="item.uuid"
      >
        <button
          @click="$emit('remove', item)"
          class="mr-2 btn btn-sm btn-danger"
        >
          <font-awesome-icon icon="trash" />
        </button>
        <div class="w-100">
          <div class="d-flex justify-content-between">
            <span
              class="h4 text-primary"
              contenteditable="contenteditable"
              @blur="
                $emit('edit', item.uuid, 'petName', $event.target.innerText)
              "
              >{{ item.petName }}</span
            >
            <span class="float-right">{{ formattedDate(item.aptDate) }}</span>
          </div>
          <div class="owner-name">
            <span class="font-weight-bold text-primary mr-1">Owner:</span>
            <span
              contenteditable="contenteditable"
              @blur="
                $emit('edit', item.uuid, 'petOwner', $event.target.innerText)
              "
              >{{ item.petOwner }}</span
            >
          </div>
          <div
            contenteditable="contenteditable"
            @blur="
              $emit('edit', item.uuid, 'aptNotes', $event.target.innerText)
            "
          >
            {{ item.aptNotes }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
import moment from "moment";

export default {
  name: "AppointList",
  props: ["appointments"],
  components: {
    FontAwesomeIcon,
  },
  methods: {
    formattedDate(aptDate) {
      return moment(new Date(aptDate)).format("MM-DD-YY, h:mm a");
    },
  },
};
</script>
<style></style>
