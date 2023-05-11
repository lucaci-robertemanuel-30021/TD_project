<template>
  <div class="submit-form">
    <div v-if="!submitted">
      <div class="form-group">
        <label for="title">Title</label>
        <input
          type="text"
          class="form-control"
          id="title"
          required
          v-model="device.title"
          name="title"
        />
      </div>

      <div class="form-group">
        <label for="description">Description</label>
        <input
          class="form-control"
          id="description"
          required
          v-model="device.description"
          name="description"
        />
      </div>

      <button @click="saveDevice" class="btn btn-success">Submit</button>
    </div>

    <div v-else>
      <h4>You submitted successfully!</h4>
      <button class="btn btn-success" @click="newDevice">Add</button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import DeviceDataService from "@/services/DeviceDataService";
import Device from "@/types/Device";
import ResponseData from "@/types/ResponseData";

export default defineComponent({
  name: "add-device",
  data() {
    return {
      device: {
        id: null,
        title: "",
        description: "",
        published: false,
      } as Device,
      submitted: false,
    };
  },
  methods: {
    saveDevice() {
      let data = {
        title: this.device.title,
        description: this.device.description,
      };

      DeviceDataService.create(data)
        .then((response: ResponseData) => {
          this.device.id = response.data.id;
          console.log(response.data);
          this.submitted = true;
        })
        .catch((e: Error) => {
          console.log(e);
        });
    },

    newDevice() {
      this.submitted = false;
      this.device = {} as Device;
    },
  },
});
</script>

<style>
.submit-form {
  max-width: 300px;
  margin: auto;
}
</style>
