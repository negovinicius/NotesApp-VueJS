<script setup>
  import {ref} from "vue";

  const showModal = ref(false);
  const newNote = ref("");
  const errorMessage = ref("")
  const notes = ref([]);

  function getRandomColor() {
   const color = "hsl(" + Math.random() * 360 + ", 100%, 75%)";
   return color; 
  }

  const addNote = () => {
    if(newNote.value.length < 10) {
      return errorMessage.value = "Note needs to be teen characters one more"
    }
      notes.value.push ({
      id: Math.floor(Math.random() * 100000 ),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomColor(),
    }),
    showModal.value = false;
    newNote.value = "";
    errorMessage.value = "";
  }
</script>
<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes"
        :key="note.id" 
        class="card" 
        :style="{backgroundColor: note.backgroundColor}">
          <p class="maint-text">{{ note.text }}</p>
          <p class="date">{{ note.date.toLocaleDateString("pt-BR") }}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }
  .container {
    max-width: 10000px;
    padding: 10px;
    margin: 0 auto;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  h1 {
    font-weight: bold;
    margin: 25px 0px 25px 100px;
    font-size: 75px;
  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: black;
    border-radius: 100%;
    color: white;
    font-size: 20px;
    margin-right: 100px;
  }
  
  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin: 20px;
    padding: 6px;
    overflow: auto;
    color: black;
  }
  .card::-webkit-scrollbar {
    background-color: transparent;
  }

  .date {
    font-size: 12.5px;
    font-weight: bold;
  }

  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.192);
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
    padding: 10px 10px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;    
  }

  .modal .close {
    background-color: red;
    margin-top: 7px;
  }

  .modal p {
    color: red;
  }
</style>