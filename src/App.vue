<script  lang="ts">
import draggable from 'vuedraggable'
export default {
  components: {
    draggable,
  },
  data() {
    return {
      drag: false,
      result: [] as Breeds[],
      responseAvailable: false,
    }
  },
  mounted() {
    //@ts-ignore
    this.fetchApiData();
  },
  methods: {
    fetchApiData() {
      fetch('https://catfact.ninja/breeds?limit=6', fetchOptions)
        .then(response => response.json())
        .then(response => response?.data)
        //@ts-ignore
        .then(response => this.result = response)
        //@ts-ignore
        .then(this.responseAvailable = true)
        .catch(err => console.error(err));

    }
  },
}
interface Breeds {
  breed: string,
  country: string,
  origin: string,
  coat: string,
  pattern: string
}

const fetchOptions = {
  method: 'GET',
  headers: {
    'accept': 'application/json',
    'X-CSRF-TOKEN': '4QPJcTZL6un1G6sx4ELa7o7DYvd374xrBZJUu0LA'
  }
}
</script>

<template>
  <div>
    <h4>Sortable Cat Breeds</h4>
    <draggable v-model="result" v-if="responseAvailable" @start="drag = true" @end="drag = false" item-key="1">
      <template #item="{ element }">
        <div class="card">
          <h5>{{ element.breed }}</h5>
          <p> {{ element.country }} </p>
          <div class="ori"> {{ element.origin }} </div>
        </div>
      </template>

    </draggable>
  </div>
</template>

<style scoped>
[data-draggable] {
  @apply flex justify-center m-3;
}

.card {
  @apply block p-6 rounded-lg shadow-lg bg-white max-w-sm;
}

p {
  @apply text-gray-700 text-base mb-4;
}

.ori {
  @apply bg-indigo-500 text-white;
}

h4 {
  @apply text-xl font-light;
}

h5 {
  @apply text-gray-900 text-xl leading-tight font-medium mb-2;
}
</style>
