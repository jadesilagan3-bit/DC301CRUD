<script setup>
import { ref, onMounted } from 'vue'
import ParkingItem from './components/ParkingItem.vue'

const spots = ref([])

const newSpotCode = ref('')
const newPlateNumber = ref('')
const newStatus = ref('Occupied')

function addSpot() {
  if (newSpotCode.value.trim() === '' || newPlateNumber.value.trim() === '') {
    return
  }

  spots.value.push({
    id: Date.now(),
    spotCode: newSpotCode.value,
    plateNumber: newPlateNumber.value,
    status: newStatus.value
  })

  newSpotCode.value = ''
  newPlateNumber.value = ''
  newStatus.value = 'Occupied'

  saveSpots()
}

function deleteSpot(id) {
  spots.value = spots.value.filter(spot => spot.id !== id)

  saveSpots()
}

function updateSpot(updatedSpot) {
  const index = spots.value.findIndex(spot => spot.id === updatedSpot.id)

  if (index !== -1) {
    spots.value[index] = updatedSpot
  }

  saveSpots()
}

function saveSpots() {
  localStorage.setItem('spots', JSON.stringify(spots.value))
}

onMounted(() => {
  const savedSpots = localStorage.getItem('spots')

  if (savedSpots) {
    spots.value = JSON.parse(savedSpots)
  }
})
</script>

<template>
  <div class="container">
    <h1>Parking Management System</h1>

    <div class="add-spot">
      <input
        v-model="newSpotCode"
        type="text"
        placeholder="Enter spot code"
        @keyup.enter="addSpot"
      />

      <input
        v-model="newPlateNumber"
        type="text"
        placeholder="Enter plate number"
        @keyup.enter="addSpot"
      />

      <select v-model="newStatus">
        <option value="Occupied">Occupied</option>
        <option value="Available">Available</option>
      </select>

      <button @click="addSpot">
        Add
      </button>
    </div>

    <div class="parking-list">
      <ParkingItem
        v-for="spot in spots"
        :key="spot.id"
        :spot="spot"
        @delete="deleteSpot"
        @update="updateSpot"
      />
    </div>
  </div>
</template>