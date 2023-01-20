<script setup>
import { ref, onMounted, computed, watch } from "vue";
const showModal = ref(false);
const newNote = ref("");
const notes = ref([]);
const errorMessage = ref("");

const name = ref("");
watch(name, (newVal) => {
  localStorage.setItem("name", newVal);
});
watch(
  notes,
  (newVal) => {
    localStorage.setItem("notes", JSON.stringify(newVal));
  },
  { deep: true }
);
onMounted(() => {
  name.value = localStorage.getItem("name") || "";
  notes.value = JSON.parse(localStorage.getItem("notes")) || [];
});
const removeNote = (note) => {
  notes.value = notes.value.filter((t) => t !== note);
};

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

const addNote = () => {
  if (newNote.value.length < 10) {
    return (errorMessage.value = "Note needs to be 10 characters or more!");
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000000),
    text: newNote.value,
    date: new Date().toDateString(),
    backgroundColor: getRandomColor(),
  });
  showModal.value = false;
  newNote.value = "";
  errorMessage = "";
};
</script>


<template>
  <main>
    <div v-show="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote()">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <div>
          <h1>Notes</h1>
          <input
            type="text"
            placeholder="your name!"
            v-model="name"
            class="name"
          />
        </div>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          class="card"
          v-for="note in notes"
          :key="note.id"
          :style="{ backgroundColor: note.backgroundColor }"
        >
        
          <button  @click="removeNote(note)">x</button>
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>
<style scoped>
main {
  height: 100vh;
  width: 100vw;
  background-color: rgb(246, 237, 209);
}
.container {
  max-width: 1000px;
  padding: 10px;
  margin: 0 auto;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
header div {
  display: flex;
  flex-wrap: wrap;
}
header input {
  border: none;
  background: none;
  margin-bottom: 25px;
  height: auto;
}
header input:focus {
  outline: none;
}
h1 {
  font-weight: bold;
  font-size: 75px;
  color: rgb(52, 52, 111);
}
header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background: rgb(52, 52, 111);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}
.card {
  width: 225px;
  height: 225px;
  background: rgb(234, 197, 104);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  /* justify-content: space-between; */
  margin-right: 20px;
  margin-bottom: 20px;
}
.date {
  font-size: 12.5px;
  font-weight: bold;
  color: gray;
  align-self: end;
  margin-top: auto;
}
.cards-container {
  display: flex;
  flex-wrap: wrap;
}
.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.modal button {
  padding: 10px 20px;
  font-size: 100%;
  width: 100%;
  background-color: rgb(97, 97, 181);
  border: none;
  color: white;
  cursor: pointer;
  margin-top: 15px;
  border-radius: 10px;
}
.modal .close {
  background-color: rgb(194, 94, 94);
  margin-top: 7px;
}
.modal p {
  color: red;
}
.card button {
  font-size: 15px;
  font-weight: bold;
  color: rgb(161, 80, 80);
  width: 20px;
  background: none;
  border: none;
  align-self: flex-end;
  cursor: pointer;
}
</style>