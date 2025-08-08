<template>
  <div id="app">
    <h1>FungiVue Ultrasound Scan Dashboard</h1>
    <p>Displaying latest mushroom growth scans</p>
    <div v-if="scans.length > 0" class="scan-grid">
      <div v-for="(scan, index) in scans" :key="index" class="scan-item">
        <img :src="scan.url" :alt="scan.name" width="300" />
        <p>{{ scan.name }} - Timestamp: {{ scan.timestamp }}</p>
      </div>
    </div>
    <p v-else>No scans available yet.</p>
    <button @click="refreshScans">Refresh Scans</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      scans: [],  // Array of { url: '/scans/scan_001.png', name: 'Scan 001', timestamp: '2025-08-07' }
    };
  },
  mounted() {
    this.loadScans();
  },
  methods: {
    async loadScans() {
      // For static files, hardcode or fetch from a JSON endpoint
      // Example: Assume a backend API at /api/scans returns JSON
      try {
        const response = await axios.get('/api/scans');
        this.scans = response.data;
      } catch (error) {
        console.error('Error loading scans:', error);
        // Fallback to static list if no API
        this.scans = [
          { url: '/scans/scan_001.png', name: 'Scan 001', timestamp: new Date().toISOString() },
          // Add more as needed
        ];
      }
    },
    refreshScans() {
      this.loadScans();
    }
  }
};
</script>

<style>
.scan-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
.scan-item { border: 1px solid #ccc; padding: 10px; text-align: center; }
</style>
