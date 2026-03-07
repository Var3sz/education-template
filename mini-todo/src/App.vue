<template>
  <div>
    <nav class="navbar navbar-dark bg-primary mb-4">
      <div class="container">
        <span class="navbar-brand mb-0 h1">Mini Todo</span>
      </div>
    </nav>

    <main class="container">
      <!-- Hibaüzenet -->
      <div v-if="error" class="alert alert-danger" role="alert">
        {{ error }}
      </div>

      <!-- Új teendő felvétele -->
      <div class="card shadow-sm mb-4">
        <div class="card-body">
          <form @submit.prevent="addTodo">
            <div class="row g-2">
              <div class="col-12 col-md-9">
                <input
                  v-model="newText"
                  type="text"
                  class="form-control"
                  placeholder="Új teendő..."
                />
              </div>
              <div class="col-12 col-md-3">
                <button type="submit" class="btn btn-success w-100">
                  Hozzáadás
                </button>
              </div>
            </div>
          </form>
        </div>
      </div>

      <!-- Statisztika -->
      <div class="card shadow-sm mb-4">
        <div class="card-header">
          <strong>Statisztika</strong>
        </div>
        <div class="card-body">
          <div class="row text-center">
            <div class="col-6 col-md-3 mb-3 mb-md-0">
              <div class="fs-4 fw-bold">{{ totalTodos }}</div>
              <div class="text-muted">Összes</div>
            </div>
            <div class="col-6 col-md-3 mb-3 mb-md-0">
              <div class="fs-4 fw-bold text-success">{{ doneCount }}</div>
              <div class="text-muted">Kész</div>
            </div>
            <div class="col-6 col-md-3">
              <div class="fs-4 fw-bold text-warning">{{ activeCount }}</div>
              <div class="text-muted">Aktív</div>
            </div>
            <div class="col-6 col-md-3">
              <div class="fs-4 fw-bold text-primary">{{ progressPercent }}%</div>
              <div class="text-muted">Készültség</div>
            </div>
          </div>
        </div>
      </div>

      <!-- Lista -->
      <div class="card shadow-sm mb-3">
        <div class="card-header d-flex justify-content-between align-items-center">
          <strong>Feladatok</strong>
          <button class="btn btn-outline-danger btn-sm" @click="clearDone">
            Kész feladatok törlése
          </button>
        </div>

        <ul v-if="todos.length" class="list-group list-group-flush">
          <TodoItem
            v-for="todo in todos"
            :key="todo.id"
            :todo="todo"
            @remove="removeTodo"
            @toggle-done="toggleDone"
          />
        </ul>

        <div v-else class="card-body text-muted text-center">
          Nincs még teendő a listában.
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue'

export default {
  name: 'App',
  components: {
    TodoItem
  },
  data() {
    return {
      todos: [
        // minta adatok opcionálisan:
        // { id: 1, text: 'Bevásárlás', done: false },
        // { id: 2, text: 'Vue gyakorlás', done: true }
      ],
      newText: '',
      error: ''
    }
  },
  computed: {
    totalTodos() {
      // TODO: összes teendő száma
      return 0
    },
    doneCount() {
      // TODO: kész teendők száma
      return 0
    },
    activeCount() {
      // TODO: aktív teendők száma
      return 0
    },
    progressPercent() {
      // TODO:
      // ha totalTodos == 0 -> 0
      // különben Math.round(doneCount / totalTodos * 100)
      return 0
    }
  },
  methods: {
    addTodo() {
      // TODO:
      // - üres szöveg ellenőrzése
      // - duplikáció ellenőrzése
      // - error beállítása
      // - új todo hozzáadása
    },
    removeTodo(id) {
      // TODO: adott id-jú teendő törlése
      console.log('removeTodo:', id)
    },
    toggleDone(id) {
      // TODO: adott id-jú teendő állapotának váltása
      console.log('toggleDone:', id)
    },
    clearDone() {
      // TODO: minden kész teendő törlése
      console.log('clearDone')
    }
  }
}
</script>