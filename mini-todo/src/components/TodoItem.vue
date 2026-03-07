<template>
  <li class="list-group-item d-flex justify-content-between align-items-center">
    <div class="d-flex align-items-center gap-2">
      <button
        class="btn btn-sm btn-outline-success"
        @click="onToggleDone"
        :title="todo.done ? 'Megjelölés aktívként' : 'Megjelölés készként'"
      >
        <i :class="todo.done ? 'bi bi-check-circle' : 'bi bi-circle'"></i>
      </button>

      <span :class="{ 'text-decoration-line-through text-muted': todo.done }">
        {{ todo.text }}
      </span>
    </div>

    <button
      class="btn btn-sm btn-outline-danger"
      @click="onRemove"
      title="Törlés"
    >
      <i class="bi bi-trash"></i>
    </button>
  </li>
</template>

<script>
export default {
  name: 'TodoItem',
  props: {
    todo: {
      type: Object,
      required: true
    }
  },
  emits: ['remove', 'toggle-done'],
  methods: {
    onToggleDone() {
      // TODO: id továbbítása a szülő felé
    },
    onRemove() {
      // TODO: id továbbítása a szülő felé
    }
  }
}
</script>