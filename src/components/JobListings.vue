<script setup>
import { RouterLink } from "vue-router";
import JobListing from "@/components/JobListing.vue";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";
import { ref, reactive, defineProps, onMounted } from "vue";
import axios from "axios";

defineProps({
  limit: Number,
  showButton: {
    type: Boolean,
    default: false,
  },
});

const jobs = ref([]);

const state = reactive({
  jobs: [],
  isLoading: true,
});

onMounted(async () => {
  try {
    const response = await axios.get("/api/jobs");
    state.jobs = response.data;
  } catch (error) {
    console.error("Error getting jobs", error);
  } finally {
    state.isLoading = false;
  }
});
</script>

<template>
  <section class="bg-blue-50 px-4 py-10 border-indigo-800">
    <div class="container-xl lg:container m-auto border-indigo-800">
      <h2 class="text-bold text-3xl font-green-500 mb-6 text-center">
        Job Listings
      </h2>

      <div v-if="state.isLoading" class="text-center text-gray-500">
        <PulseLoader />
      </div>
      <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <JobListing
          v-for="job in state.jobs.slice(0, limit || state.jobs.length)"
          :key="job.id"
          :job="job"
        />
      </div>
    </div>
  </section>

  <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
    <RouterLink
      to="/jobs"
      class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
      >View All Jobs</RouterLink
    >
  </section>
</template>
