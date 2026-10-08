<script setup>
import { ref } from 'vue'

const props = defineProps({
  spot: Object
})

const emit = defineEmits([
  'delete',
  'update'
])

const isEditing = ref(false)

const editedSpotCode = ref(props.spot.spotCode)
const editedPlateNumber = ref(props.spot.plateNumber)
const editedStatus = ref(props.spot.status)

function updateSpot() {
  if (editedSpotCode.value.trim() === '' || editedPlateNumber.value.trim() === '') {
    return
  }

  emit('update', {
    ...props.spot,
    spotCode: editedSpotCode.value,
    plateNumber: editedPlateNumber.value,
    status: editedStatus.value
  })

  isEditing.value = false
}

function deleteSpot() {
  emit('delete', props.spot.id)
}

function startEditing() {
  editedSpotCode.value = props.spot.spotCode
  editedPlateNumber.value = props.spot.plateNumber
  editedStatus.value = props.spot.status
  isEditing.value = true
}
</script>

<template>
  <div class="parking-item">
    <div v-if="!isEditing">
      <span>{{ spot.spotCode }}</span>
      <span>{{ spot.plateNumber }}</span>
      <span>{{ spot.status }}</span>

      <button @click="startEditing">
        Edit
      </button>

      <button @click="deleteSpot">
        Delete
      </button>
    </div>

    <div v-else>
      <input
        v-model="editedSpotCode"
        placeholder="Spot Code"
        @keyup.enter="updateSpot"
      />

      <input
        v-model="editedPlateNumber"
        placeholder="Plate Number"
        @keyup.enter="updateSpot"
      />

      <select v-model="editedStatus">
        <option value="Occupied">Occupied</option>
        <option value="Available">Available</option>
      </select>

      <button @click="updateSpot">
        Save
      </button>

      <button @click="isEditing = false">
        Cancel
      </button>
    </div>
  </div>
</template>