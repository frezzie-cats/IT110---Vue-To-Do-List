<template>
  <v-app>
    <v-container class="app-container" max-width="600px">
      <v-card class="app-card">
        <v-card-title class="text-h5" style="color: #bb86fc; font-weight: bold;">
          To-Do List Scheduler
        </v-card-title>

        <v-card-text>
          <v-form v-model="formValid">
            <!-- Task Title -->
            <v-text-field 
              v-model="newTask.title" 
              label="Task Title" 
              required
              outlined
              dense
              dark
            ></v-text-field>

            <!-- Due Date as Text Field -->
            <v-text-field 
              v-model="newTask.dueDate" 
              label="Due Date (YYYY-MM-DD)" 
              required
              outlined
              dense
              dark
            ></v-text-field>

            <!-- Priority Selection -->
            <v-select
              v-model="newTask.priority"
              :items="priorities"
              label="Priority"
              required
              outlined
              dense
              dark
            ></v-select>
          </v-form>
        </v-card-text>

        <!-- Add Task Button -->
        <v-card-actions>
          <v-btn color="primary" @click="addTask" :disabled="!formValid" class="add-task-btn" block> 
            <span style="color: black;">Add Task</span>
          </v-btn>
        </v-card-actions>
      </v-card>

      <!-- Task List -->
      <v-list>
        <v-list-item 
          v-for="task in tasks" 
          :key="task.id"
          class="task-item"
        >
          <v-list-item-content>
            <v-list-item-title class="task-title" :class="{ 'completed': task.completed }">
              {{ task.title }}
            </v-list-item-title>
            <v-list-item-subtitle>
              Due: {{ formatDate(task.dueDate) }} - Priority: {{ task.priority }}
            </v-list-item-subtitle>
          </v-list-item-content>

          <!-- Right-Aligned Controls (Checkbox and Delete Button) -->
          <v-list-item-action>
            <!-- Checkbox for marking task as completed -->
            <v-checkbox 
              v-model="task.completed" 
              color="green" 
              dense 
              class="task-checkbox" 
              size="small" 
            ></v-checkbox>

            <!-- Delete Button -->
            <v-btn 
              icon 
              @click="deleteTask(task.id)" 
              class="delete-btn" 
              small 
            >
              <v-icon color="red">mdi-delete</v-icon>
            </v-btn>
          </v-list-item-action>
        </v-list-item>
      </v-list>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      formValid: false,
      newTask: {
        title: '',
        dueDate: '',
        priority: '',
        completed: false
      },
      priorities: ['Low', 'Medium', 'High'],
      tasks: []
    };
  },
  methods: {
    addTask() {
      if (this.newTask.title && this.newTask.dueDate && this.newTask.priority) {
        // Add task with a unique ID (using timestamp for simplicity)
        this.tasks.push({ ...this.newTask, id: Date.now() });
        // Reset task fields after adding
        this.newTask = { title: '', dueDate: '', priority: '', completed: false };
      }
    },
    deleteTask(id) {
      // Remove task by filtering out the one with the matching ID
      this.tasks = this.tasks.filter(task => task.id !== id);
    },
    formatDate(date) {
      // Format the date as MM/DD/YYYY for better readability
      const d = new Date(date);
      return `${d.getMonth() + 1}/${d.getDate()}/${d.getFullYear()}`;
    }
  }
};
</script>

<style scoped>
/* Dark Theme Container and Card Styling */
.app-container {
  margin-top: 30px;
  background-color: #121212;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
  padding: 20px;
}

.app-card {
  background-color: #1e1e1e;
  border-radius: 10px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.5);
  padding: 20px;
  margin-bottom: 20px;
}

/* Task Item Styling */
.task-item {
  background-color: #2a2a2a;
  border-radius: 8px;
  margin-bottom: 10px;
  padding: 10px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
}

/* Completed Task Strikethrough */
.task-title.completed {
  text-decoration: line-through;
  color: #9e9e9e;
}

/* Add Task Button */
.add-task-btn {
  font-weight: bold;
  border-radius: 5px;
  background-color: #bb86fc;
}

/* Checkbox and Delete Button Styling */
.v-list-item-action {
  display: flex;
  align-items: center; /* Aligns controls to the center vertically */
  justify-content: flex-end; /* Aligns controls to the right */
}

.task-checkbox {
  margin-right: 12px; /* Spacing between checkbox and delete button */
  width: 25px; /* Set specific width */
  height: 55px; /* Set specific height */
  align-self: center; /* Ensure checkbox is centered */
}

.delete-btn {
  margin-left: 8px; /* Spacing between checkbox and delete button */
  font-size: 0.8rem; /* Smaller font size */
  width: 24px; /* Set specific width */
  height: 24px; /* Set specific height */
  align-self: center; /* Ensure delete button is centered */
}

/* Text and Input Styling */
.v-text-field, .v-select {
  color: white;
}
</style>
