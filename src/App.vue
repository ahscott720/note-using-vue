<script setup>
import { ref } from "vue";
import { uuid } from "vue-uuid"

const modalShow = ref(false);
const noteMessage = ref("");
const notesCollection = ref([]);
const errorMessage = ref("");

function getDarkColor() {
    var color = '#';
    for (var i = 0; i < 6; i++) {
        color += Math.floor(Math.random() * 10);
    }
    return color;
}

const addNote = () => {
  if (noteMessage.value.length < 12){
    return errorMessage.value = "The input should be greater than 12 char!"
  }
  notesCollection.value.push({
    id: uuid.v4(),
    text: noteMessage.value,
    date: new Date().toLocaleDateString(),
    color: getDarkColor()
  })
  modalShow.value = false;
  noteMessage.value = "";
  errorMessage.value = ""
}

</script>

<template>
  <main>
    <div v-if="modalShow" class="modal">
      <div>
        <textarea v-model.trim="noteMessage" rows="10" cols="50"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button class="add" @click="addNote">Add Note</button>
        <button class="close" @click="modalShow = false">Close</button>
      </div>
    </div>
    <div class="container">
      <div class="title">
        <h1>Note</h1>
        <button @click="modalShow = true">+</button>
      </div>
      <div class="note-container">
        <div v-for="note in notesCollection" :key="note.id" class="card" :style="{ backgroundColor: note.color }">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  background-color: #A3C7D6;
  width: 100%;
  height: 100%;
}

.modal {
  position: absolute;
  width: 100vw;
  height: 100vh;
  background-color: rgba(98,79,130,0.8);
  z-index:2;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal div {
  background-color: #A3C7D6;
  border-radius: 15px;
  margin: auto;  
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.modal p{
  color: #75208d;
}
.add {
  height: 30px;
  font-size: 20px;
  font-weight: bold;
  background-color: #9F73AB;
  color: #efeff7;
  margin-top: 10px;
  cursor: pointer;
}

.close {
  height: 30px;
  font-size: 20px;
  font-weight: bold;
  background-color: #75208d;
  color: #efeff7;
  margin-top: 10px;
  cursor: pointer;
}
.container {
  max-width: 80vw;
  height: 100vh;
  margin: 0 auto;
  background-color: #A3C7D6; 
}

.title {
  display: flex;
  justify-content: space-evenly;
  
}

.title h1 {
  color: #3F3B6C;
  font-weight: bolder;
  font-size: 50px;
  line-height: 50px;
  margin: 20px;
}

.title button {
  background-color: #3F3B6C;
  color: white;
  font-weight: bolder;
  font-size: 35px;
  width: 50px;
  height: 50px;
  border-radius: 100%;
  margin: 20px;
  cursor: pointer;
}

.note-container {
  display: flex;
  flex-wrap: wrap;
}
.card {
  display: flex;
  width: 150px;
  height: 150px;
  border-radius: 10px;
  background-color: aqua;
  color:white;
  padding: 5px 5px;
  margin: 10px;
}

.main-text {
  font-size: 14px;  
}
.date {
  font-size: 10px;
  position:absolute;
  bottom: 0;
  left: 5px;
}

</style>
