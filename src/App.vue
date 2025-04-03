<template>
  <div class="min-h-screen bg-gradient-to-br from-purple-600 to-blue-500 p-8 flex items-center justify-center">
    <div class="max-w-2xl w-full bg-white rounded-2xl shadow-2xl p-8">
      <h1 class="text-4xl font-extrabold text-gray-800 mb-8 text-center">Todo list</h1>

      <div class="mb-4 text-center text-lg">
        <p class="font-semibold text-gray-600">Broj zadataka: {{ tasks.length }}</p>
      </div>

      <div class="flex mb-6 justify-center">
        <input 
          v-model="newTask" 
          @keyup.enter="addTask" 
          placeholder="Add new task..."
          class="flex-1 p-4 text-lg border border-gray-300 rounded-l-xl focus:outline-none focus:ring-2 focus:ring-purple-500"
        />
        <button
          @click="addTask"
          class="bg-purple-600 text-white px-6 py-4 rounded-r-xl hover:bg-purple-800 transition-all"
        >
          ➕ Add
        </button>
      </div>

      <p v-if="tasks.length === 0" class="text-center text-lg text-gray-500">
        Nema zadataka. Dodaj neki!
      </p>

      <ul v-if="tasks.length > 0" class="space-y-4">
        <li v-for="(task, index) in tasks" :key="index" class="flex items-center justify-between p-4 rounded-xl shadow-sm">
          <div class="text-lg">{{ task.text }}</div>
          <div class="space-x-3">
            <button @click="editTask(index)" class="text-blue-500 hover:text-blue-700 text-xl">✏️</button>
            <button @click="deleteTask(index)" class="text-red-500 hover:text-red-700 text-xl">❌</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
  <FooterComponent />
</template>

<script>
import { ref, onMounted } from 'vue';
import FooterComponent from "./components/FooterComponent.vue";

export default {
  components: { FooterComponent },
  setup() {
    const newTask = ref("");
    const tasks = ref([]);

    const addTask = () => {
      if (newTask.value.trim() !== "") {
        tasks.value.push({ text: newTask.value });
        newTask.value = "";
      }
    };

    const editTask = (index) => {
      const updatedText = prompt("Edit task", tasks.value[index].text);
      if (updatedText !== null) {
        tasks.value[index].text = updatedText;
      }
    };

    const deleteTask = (index) => {
      tasks.value.splice(index, 1);
    };

    onMounted(() => {
      alert("Mounted: Stranica je učitana, ali podaci će se izgubiti nakon refreshanja!");
    });

    return { newTask, tasks, addTask, editTask, deleteTask };
  }
};
</script>
