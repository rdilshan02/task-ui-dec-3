<!-- eslint-disable vue/html-indent -->
<template>
  <div class="w-full p-1">
    <div class="overflow-x-auto">
      <h2 class="text-center">{{ currentTime }}</h2>

      <div class="grid md:grid-cols-3">
        <div
          class="flex justify-center"
          v-for="row in rows"
          :key="row.deviceKey"
        >
          <div
            class="bg-white shadow rounded-lg mb-2 text-xl text-dark font-bold border-3 device-card p-1 cursor-pointer"
            @click="openChart(row.deviceKey)"
          >
            <h3 class="text-center mb-0">
              {{ row.location }}
            </h3>
            <div class="flex flex-row justify-between items-center mt-1">
              <img
                :id="`fuel-tank-${row.deviceKey}`"
                class="fuel-tank-icon"
                :class="row.deviceKey"
                :src="require('@/assets/images/diagrams/icon.png')"
              />
              <div class="flex flex-col justify-center items-center">
                <div>Fill Percentage</div>
                <div class="text-center">{{ row.fillPercentage }}</div>
              </div>
            </div>
            
            <div class="flex flex-row justify-between items-center mt-1">
              <div class="flex flex-col justify-center items-center">
                <img
                  class="w-8"
                  :src="appIcons['battery-' + row.batteryStatus]"
                />
                <p class="mb-0">{{ row.battery }}</p>
              </div>
              <div class="flex flex-col justify-center items-center">
                <img
                  class="w-8"
                  :src="appIcons['wifi-' + row.communicationStatus]"
                />
                <p class="mb-0">{{ formatTime(row.time) }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import appIcons from "@/@leecom/app-icons";
import InlineSvg from "vue-inline-svg";

export default {
  components: {
    InlineSvg,
  },
  data() {
    return {
      rows: [],
      MODULE_NAME: "summaryModule",
      key: this.$moment().valueOf(),
      appIcons,
      lastUpdated: "loading...",
      currentTime: "loading..",
    };
  },
  methods: {
    fetchTableData() {

      const mockData = {
        results: [
          {
            location: "DD6 (Digdola Big Pond)",
            fillPercentage: "_ _",
            battery: "_ _",
            time: "2025-01-25 09:21:15",
            communicationStatus: "red",
            batteryStatus: "gray",
            deviceKey: "6isrqimgjj8k",
            changeIndicator: "down"
          },
          {
            location: "DD7 (Digdola Stock Pond)",
            fillPercentage: "_ _",
            battery: "_ _",
            time: "2025-01-25 09:21:15",
            communicationStatus: "red",
            batteryStatus: "gray",
            deviceKey: "jlk45gfd88hk",
            changeIndicator: "down"
          },
          {
            location: "DD11 (Digdola Boundary Pond)",
            fillPercentage: "_ _",
            battery: "_ _",
            time: "2025-01-25 09:21:15",
            communicationStatus: "red",
            batteryStatus: "gray",
            deviceKey: "98fhgk54lmn0",
            changeIndicator: "down"
          },
          {
            location: "MNC 2 (MNC Big Pond)",
            fillPercentage: "0 %",
            battery: "3.84",
            time: "2025-10-19 22:09:00",
            communicationStatus: "red",
            batteryStatus: "green",
            deviceKey: "b67dfhgnc934",
            changeIndicator: null
          },
          {
            location: "MNC 3 (MNC Stock Pond)",
            fillPercentage: "_ _",
            battery: "_ _",
            time: "2025-01-25 09:21:16",
            communicationStatus: "red",
            batteryStatus: "gray",
            deviceKey: "x9fg46jklzop",
            changeIndicator: null
          },
          {
            location: "Mill 1500 Pond",
            fillPercentage: "_ _",
            battery: "_ _",
            time: "2025-01-25 09:21:16",
            communicationStatus: "red",
            batteryStatus: "gray",
            deviceKey: "qwe89tyui56z",
            changeIndicator: "down"
          },
          {
            location: "Mill 750 Pond",
            fillPercentage: "_ _",
            battery: "_ _",
            time: "2025-01-25 09:21:16",
            communicationStatus: "red",
            batteryStatus: "gray",
            deviceKey: "ghr45yplq290",
            changeIndicator: "down"
          }
        ]
      };

     
      this.rows = mockData.results;


    },
    formatTime(timeString) {
      
      return this.$moment(timeString).format("YYYY-MM-DD HH:mm:ss");
    },
    updateTime() {
      this.currentTime = this.$moment().format("HH:mm:ss  DD MMMM YYYY");
    },
    openChart(deviceKey) {
      this.$router.push({
        name: "tanks",
        query: {
          deviceKey,
        },
      });
    },
    updateLevelIndicator(deviceKey, fillPercentage) {
      const rect = document.querySelector(
        `#fuel-tank-${deviceKey} #level-indicator`
      );
      if (rect) {
        const maxHeight = 115; 
        
        
        const level = parseInt(fillPercentage);
        const displayLevel = Math.min(level, 100); 

        const calculatedHeight = maxHeight - (displayLevel / 100) * maxHeight;

        rect.style.height = `${calculatedHeight}px`;
      }
    },
    svgMounted(event) {
      this.rows.forEach((row) => {
        this.updateLevelIndicator(row.deviceKey, row.fillPercentage);
      });
    },
  },
  created() {
    this.fetchTableData();
    this.updateTime();
    setInterval(() => {
      this.updateTime();
    }, 1000);
  },
};
</script>

<style scoped>
@media (min-width: 767px) {
  .md\:block {
    display: block !important;
    visibility: visible;
  }
}

.device-card {
  border-color: #000000;
  max-width: 300px;
}

.fuel-tank-icon {
  max-height: 78px;
}
</style>