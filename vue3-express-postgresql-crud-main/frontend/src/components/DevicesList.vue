<template>
  <div class="list row">
    <div class="col-md-8">
      <div class="input-group mb-3">
        <input
          type="text"
          class="form-control"
          placeholder="Search by title"
          v-model="title"
        />
        <div class="input-group-append">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="searchTitle"
          >
            Search
          </button>
        </div>
      </div>
    </div>
    <div class="col-md-6">
      <h4>Devices List</h4>
      <ul class="list-group">
        <li
          class="list-group-item"
          :class="{ active: index == currentIndex }"
          v-for="(device, index) in devices"
          :key="index"
          @click="setActiveDevice(device, index)"
        >
          {{ device.title }}
        </li>
      </ul>

      <button class="m-3 btn btn-sm btn-danger" @click="removeAllDevices">
        Remove All
      </button>
    </div>
    <div class="col-md-6">
      <div v-if="currentDevice.id">
        <h4>Device</h4>
        <div>
          <label><strong>Title:</strong></label> {{ currentDevice.title }}
        </div>
        <div>
          <label><strong>Description:</strong></label>
          {{ currentDevice.description }}
        </div>
        <div>
          <label><strong>Price:</strong></label>
          {{ currentDevice.price }}
        </div>
        <div>
          <label><strong>Brand:</strong></label>
          {{ currentDevice.brand }}
        </div>
        <div>
          <label><strong>Condition:</strong></label>
          {{ currentDevice.condition }}
        </div>
        <div>
          <label><strong>Available:</strong></label>
          {{ currentDevice.available }}
        </div>
        <div>
          <label><strong>In stock:</strong></label>
          {{ currentDevice.in_stock }}
        </div>
        <div>
          <label><strong>Warranty:</strong></label>
          {{ currentDevice.warranty }}
        </div>
        <div>
          <label><strong>Delivery options:</strong></label>
          {{ currentDevice.delivery_options }}
        </div>
        <div>
          <label><strong>Status:</strong></label>
          {{ currentDevice.published ? "Published" : "Pending" }}
        </div>

        <router-link
          :to="'/devices/' + currentDevice.id"
          class="badge badge-warning"
          >Edit</router-link
        >
      </div>
      <div v-else>
        <br />
        <p>Please click on a Device...</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import DeviceDataService from "@/services/DeviceDataService";
import Device from "@/types/Device";
import ResponseData from "@/types/ResponseData";

export default defineComponent({
  name: "devices-list",
  data() {
    return {
      devices: [] as Device[],
      currentDevice: {} as Device,
      currentIndex: -1,
      title: "",
    };
  },
  methods: {
    retrieveDevices() {
      DeviceDataService.getAll()
        .then((response: ResponseData) => {
          this.devices = response.data;
          console.log(response.data);
        })
        .catch((e: Error) => {
          console.log(e);
        });
    },

    refreshList() {
      this.retrieveDevices();
      this.currentDevice = {} as Device;
      this.currentIndex = -1;
    },

    setActiveDevice(device: Device, index = -1) {
      this.currentDevice = device;
      this.currentIndex = index;
    },

    removeAllDevices() {
      DeviceDataService.deleteAll()
        .then((response: ResponseData) => {
          console.log(response.data);
          this.refreshList();
        })
        .catch((e: Error) => {
          console.log(e);
        });
    },

    searchTitle() {
      DeviceDataService.findByTitle(this.title)
        .then((response: ResponseData) => {
          this.devices = response.data;
          this.setActiveDevice({} as Device);
          console.log(response.data);
        })
        .catch((e: Error) => {
          console.log(e);
        });
    },
  },
  mounted() {
    this.retrieveDevices();
  },
});
</script>

<style>
.list {
  text-align: left;
  max-width: 750px;
  margin: auto;
}
</style>
