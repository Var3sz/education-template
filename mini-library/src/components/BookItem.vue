<template>
  <li class="list-group-item d-flex justify-content-between align-items-center">

    <!-- Könyv adatai -->
    <div>
      <strong>{{ book.title }}</strong>
      <br>
      <small class="text-muted">
        Kölcsönző: {{ book.borrower }}
      </small>
    </div>


    <!-- Gombok -->
    <div class="btn-group">

      <!-- Állapot váltás -->
      <button
        class="btn btn-sm btn-outline-success"
        @click="onToggleReturned"
      >
        <i v-if="book.returned" class="bi bi-book-fill"></i>
        <i v-else class="bi bi-book"></i>
      </button>

      <!-- Törlés -->
      <button
        class="btn btn-sm btn-outline-danger"
        @click="onRemove"
      >
        <i class="bi bi-trash"></i>
      </button>

    </div>

  </li>
</template>