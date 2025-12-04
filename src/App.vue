<template>
  <div class="app-container">
    <header class="app-header">
      <h1>FlexZone Fitness</h1>
      <p class="subtitle">Class Scheduler</p>
    </header>

    <main class="app-main">
      <div class="container">
        <!-- Add Class Form Section -->
        <section class="form-section">
          <h2>Add New Class</h2>
          <form @submit.prevent="addClass" class="form">
            <div class="form-group">
              <label for="className">Class Name</label>
              <input
                id="className"
                v-model="newClass.name"
                type="text"
                placeholder="e.g., Yoga, Pilates, Spinning"
                class="form-input"
              />
              <span v-if="submitted && !newClass.name" class="error">Class name is required</span>
            </div>

            <div class="form-group">
              <label for="coach">Coach Name</label>
              <input
                id="coach"
                v-model="newClass.coach"
                type="text"
                placeholder="e.g., Tyhiesha Johnson"
                class="form-input"
              />
              <span v-if="submitted && !newClass.coach" class="error">Coach name is required</span>
            </div>

            <div class="form-row">
              <div class="form-group">
                <label for="date">Date</label>
                <input
                  id="date"
                  v-model="newClass.date"
                  type="date"
                  class="form-input"
                />
                <span v-if="submitted && !newClass.date" class="error">Date is required</span>
              </div><br>

              <div class="form-group">
                <label for="time">Time</label>
                <input
                  id="time"
                  v-model="newClass.time"
                  type="time"
                  class="form-input"
                />
                <span v-if="submitted && !newClass.time" class="error">Time is required</span>
              </div>
            </div>

            <div class="form-group">
              <label for="capacity">Capacity</label>
              <input
                id="capacity"
                v-model.number="newClass.capacity"
                type="number"
                placeholder="Maximum participants"
                min="1"
                class="form-input"
              />
              <span v-if="submitted && (!newClass.capacity || newClass.capacity < 1)" class="error">
                Capacity must be at least 1
              </span>
            </div>

            <button type="submit" class="btn btn-primary">Add Class</button>
          </form>
        </section>

        <!-- Schedule List Section -->
        <section class="schedule-section">
          <h2>
            Scheduled Classes
            <span class="badge">{{ totalClasses }}</span>
          </h2>

          <!-- Empty State -->
          <div v-if="classes.length === 0" class="empty-state">
            <p> No sessions scheduled yet.</p>
            <p>Use the form above to add your first class</p>
          </div>

          <!-- Classes List -->
          <div v-else class="classes-list">
            <div
              v-for="fitnessClass in classes"
              :key="fitnessClass.id"
              class="class-card"
            >
              <div class="class-header">
                <h3>{{ fitnessClass.name }}</h3>
                <button
                  @click="deleteClass(fitnessClass.id)"
                  class="btn btn-delete"
                  title="Delete class"
                >
                  ✕
                </button>
              </div>

              <div class="class-details">
                <div class="detail-item">
                  <span class="label">Coach:</span>
                  <span class="value">{{ fitnessClass.coach }}</span>
                </div>
                <div class="detail-item">
                  <span class="label">Date:</span>
                  <span class="value">{{ formatDate(fitnessClass.date) }}</span>
                </div>
                <div class="detail-item">
                  <span class="label">Time:</span>
                  <span class="value">{{ fitnessClass.time }}</span>
                </div>
                <div class="detail-item">
                  <span class="label">Capacity:</span>
                  <span class="value">{{ fitnessClass.capacity }} participants</span>
                </div>
              </div>
            </div>
          </div>
        </section>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Data
const classes = ref([])
const submitted = ref(false)
const nextId = ref(1)

const newClass = ref({
  name: '',
  coach: '',
  date: '',
  time: '',
  capacity: null
})

// Computed property for total class count
const totalClasses = computed(() => classes.value.length)

// Method to validate form
const isFormValid = () => {
  return (
    newClass.value.name.trim() !== '' &&
    newClass.value.coach.trim() !== '' &&
    newClass.value.date !== '' &&
    newClass.value.time !== '' &&
    newClass.value.capacity && newClass.value.capacity > 0
  )
}

// Method to add a new class
const addClass = () => {
  submitted.value = true

  if (!isFormValid()) {
    return
  }

  classes.value.push({
    id: nextId.value++,
    ...newClass.value
  })

  // Reset form
  newClass.value = {
    name: '',
    coach: '',
    date: '',
    time: '',
    capacity: null
  }
  submitted.value = false
}

// Method to delete a class
const deleteClass = (id) => {
  classes.value = classes.value.filter(fitnessClass => fitnessClass.id !== id)
}

// Method to format date for display
const formatDate = (dateString) => {
  const options = { weekday: 'short', month: 'short', day: 'numeric', year: 'numeric' }
  return new Date(dateString).toLocaleDateString('en-US', options)
}
</script>/* ===== GLOBAL STYLES ===== */



<style>
body {
  margin: 0;
  padding: 0;
  font-family: "Inter", sans-serif;
  background: 
    linear-gradient(rgba(0,0,0,0.6), rgba(0,0,0,0.6)),
    url("https://images.pexels.com/photos/1954524/pexels-photo-1954524.jpeg") 
    no-repeat center/cover fixed;
  color: #fff;
  min-height: 100vh;
}

.app-container {
  padding: 20px;
}

.app-header {
  text-align: center;
  margin-bottom: 30px;
}

.app-header h1 {
  font-size: 36px;
  font-weight: 800;
  letter-spacing: 1px;
  text-transform: uppercase;
}

.subtitle {
  margin-top: -8px;
  font-size: 16px;
  opacity: 0.85;
}

.container {
  max-width: 1000px;
  margin: 0 auto;
}

.form-section {
  background: rgba(255,255,255,0.1);
  backdrop-filter: blur(6px);
  padding: 25px;
  border-radius: 14px;
  margin-bottom: 40px;
  border: 1px solid rgba(255,255,255,0.15);
}

.form-section h2 {
  font-size: 22px;
  margin-bottom: 18px;
  font-weight: 600;
}

.form {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.form-row {
  display: flex;
  gap: 16px;
}

.form-group label {
  font-weight: 600;
  margin-bottom: 6px;
}

.form-input {
  width: 100%;
  padding: 12px;
  border-radius: 8px;
  border: none;
  font-size: 15px;
  background: rgba(255,255,255,0.9);
}

.form-input:focus {
  outline: 2px solid #3ba3ff;
  background: #ffffff;
}

.error {
  font-size: 13px;
  color: #ff6b6b;
  margin-top: 4px;
}

.btn-primary {
  padding: 12px;
  border: none;
  background: #3ba3ff;
  color: white;
  font-weight: bold;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.2s;
}

.btn-primary:hover {
  background: #1f8eeb;
}


.schedule-section {
  background: rgba(255,255,255,0.1);
  padding: 25px;
  border-radius: 14px;
  backdrop-filter: blur(5px);
  border: 1px solid rgba(255,255,255,0.15);
}

.schedule-section h2 {
  font-size: 24px;
  display: flex;
  align-items: center;
  gap: 10px;
}

.badge {
  background: #3ba3ff;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: bold;
}


.empty-state {
  text-align: center;
  padding: 40px 10px;
  font-size: 18px;
  opacity: 0.9;
}


.classes-list {
  margin-top: 20px;
  display: grid;
  gap: 20px;
}

.class-card {
  background: rgba(255,255,255,0.15);
  padding: 18px;
  border-radius: 14px;
  backdrop-filter: blur(6px);
  border-left: 4px solid #3ba3ff;
  transition: 0.25s ease;
}

.class-card:hover {
  transform: translateY(-4px);
  background: rgba(255,255,255,0.25);
}

.class-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.class-header h3 {
  margin: 0;
  font-size: 22px;
  font-weight: 700;
}

.btn-delete {
  background: #ff4d4d;
  border: none;
  color: white;
  font-size: 16px;
  padding: 4px 10px;
  border-radius: 8px;
  cursor: pointer;
  transition: 0.2s;
}

.btn-delete:hover {
  background: #d93636;
}

.class-details {
  margin-top: 15px;
  display: grid;
  gap: 8px;
  font-size: 15px;
}

.detail-item .label {
  font-weight: bold;
  margin-right: 6px;
}

.value {
  opacity: 0.9;
}


@media (max-width: 768px) {
  .form-row {
    flex-direction: column;
  }

  .class-card {
    padding: 15px;
  }
}</style>