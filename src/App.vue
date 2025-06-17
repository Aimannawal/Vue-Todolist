<template>
  <div class="min-h-screen bg-gray-50 py-8">
    <div class="container mx-auto max-w-2xl px-4">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-gray-900 mb-2">📝 ToDo List</h1>
        <p class="text-gray-600">Organize your tasks with style</p>
      </div>

      <!-- Add Note Form -->
      <div class="bg-white rounded-xl shadow-sm border border-gray-200 p-6 mb-6">
        <form @submit.prevent="addNote" class="flex gap-3">
          <input 
            v-model="newNote" 
            type="text" 
            placeholder="What needs to be done?" 
            class="flex-1 px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-gray-900 focus:border-transparent transition-all duration-200 text-gray-900 placeholder-gray-500"
          />
          <button 
            type="submit"
            class="px-6 py-3 bg-gray-900 text-white rounded-lg hover:bg-gray-800 focus:outline-none focus:ring-2 focus:ring-gray-900 focus:ring-offset-2 transition-all duration-200 font-medium"
          >
            Add Task
          </button>
        </form>
      </div>

      <!-- Notes List -->
      <div class="space-y-3">
        <div v-if="notes.length === 0" class="text-center py-12">
          <div class="text-6xl mb-4">📋</div>
          <p class="text-gray-500 text-lg">No tasks yet. Add one above!</p>
        </div>
        
        <div 
          v-for="note in notes" 
          :key="note.id" 
          class="bg-white rounded-xl shadow-sm border border-gray-200 p-5 hover:shadow-md transition-all duration-200 group"
        >
          <div class="flex justify-between items-start">
            <div class="flex-1">
              <p class="text-gray-900 font-medium text-lg mb-2">{{ note.title }}</p>
              <div class="flex items-center text-sm text-gray-500">
                <svg class="w-4 h-4 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                </svg>
                {{ formatDate(note.created_at) }}
              </div>
            </div>
            <button 
              @click="deleteNote(note.id)"
              class="ml-4 p-2 text-gray-400 hover:text-red-500 hover:bg-red-50 rounded-lg transition-all duration-200 opacity-0 group-hover:opacity-100"
              title="Delete task"
            >
              <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"></path>
              </svg>
            </button>
          </div>
        </div>
      </div>

      <!-- Footer -->
      <div class="text-center mt-12 text-gray-500 text-sm">
        <p>{{ notes.length }} {{ notes.length === 1 ? 'task' : 'tasks' }} total</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      api: "http://localhost:8080/notes", 
      notes: [],
      newNote: ""
    };
  },
  methods: {
    async fetchNotes() {
      try {
        const res = await fetch(this.api);
        this.notes = await res.json();
      } catch (error) {
        console.error('Error fetching notes:', error);
      }
    },
    async addNote() {
      if (!this.newNote.trim()) return;
      
      try {
        const res = await fetch(this.api, {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ title: this.newNote.trim() })
        });
        const data = await res.json();
        this.notes.push(data);
        this.newNote = "";
      } catch (error) {
        console.error('Error adding note:', error);
      }
    },
    async deleteNote(id) {
      try {
        const res = await fetch(`${this.api}/${id}`, {
          method: "DELETE"
        });
        if (res.ok) {
          this.notes = this.notes.filter(n => n.id !== id);
        }
      } catch (error) {
        console.error('Error deleting note:', error);
      }
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      const now = new Date();
      const diffInHours = Math.floor((now - date) / (1000 * 60 * 60));
      
      if (diffInHours < 1) {
        return 'Just now';
      } else if (diffInHours < 24) {
        return `${diffInHours} hour${diffInHours > 1 ? 's' : ''} ago`;
      } else {
        return date.toLocaleDateString('en-US', { 
          month: 'short', 
          day: 'numeric',
          hour: '2-digit',
          minute: '2-digit'
        });
      }
    }
  },
  mounted() {
    this.fetchNotes();
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@300;400;500;600;700&display=swap');

* {
  font-family: 'Raleway', sans-serif;
}

body {
  background-color: #f9fafb;
  margin: 0;
  padding: 0;
}

::-webkit-scrollbar {
  width: 6px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: #a8a8a8;
}
</style>