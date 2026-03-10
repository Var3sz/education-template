<template>
  <div class="container py-4">

    <!-- Navbar -->
    <nav class="navbar navbar-dark bg-dark rounded mb-4">
      <div class="container-fluid">
        <span class="navbar-brand mb-0 h1">Mini Library</span>
      </div>
    </nav>


    <!-- Hibaüzenet -->
    <div v-if="error" class="alert alert-danger">
      {{ error }}
    </div>


    <!-- Új könyv felvétele -->
    <div class="card mb-4">
      <div class="card-header">
        Új könyv felvétele
      </div>

      <div class="card-body">

        <div class="row g-2">

          <div class="col-md-5">
            <input
              type="text"
              class="form-control"
              placeholder="Könyv címe"
              v-model="newTitle"
            >
          </div>

          <div class="col-md-5">
            <input
              type="text"
              class="form-control"
              placeholder="Kölcsönző neve"
              v-model="newBorrower"
            >
          </div>

          <div class="col-md-2 d-grid">
            <button class="btn btn-primary" @click="addBook">
              Hozzáadás
            </button>
          </div>

        </div>

      </div>
    </div>


    <!-- Szűrők -->
    <div class="mb-4">

      <div class="btn-group">

        <button
          class="btn btn-outline-secondary"
          @click="setFilter('all')"
        >
          Összes
        </button>

        <button
          class="btn btn-outline-secondary"
          @click="setFilter('borrowed')"
        >
          Kölcsönben
        </button>

        <button
          class="btn btn-outline-secondary"
          @click="setFilter('returned')"
        >
          Visszahozott
        </button>

      </div>

    </div>


    <!-- Statisztika -->
    <div class="card mb-4">

      <div class="card-header">
        Statisztika
      </div>

      <div class="card-body">

        <div class="row text-center">

          <div class="col-md-3">
            <h5>Összes könyv</h5>
            <p class="fs-4">{{ totalBooks }}</p>
          </div>

          <div class="col-md-3">
            <h5>Visszahozott</h5>
            <p class="fs-4">{{ returnedCount }}</p>
          </div>

          <div class="col-md-3">
            <h5>Kölcsönben</h5>
            <p class="fs-4">{{ borrowedCount }}</p>
          </div>

          <div class="col-md-3">
            <h5>Arány</h5>
            <p class="fs-4">{{ returnedPercent }}%</p>
          </div>

        </div>

      </div>

    </div>


    <!-- Könyvek listája -->
    <div class="card">

      <div class="card-header d-flex justify-content-between align-items-center">

        <span>Könyvek listája</span>

        <button
          class="btn btn-sm btn-danger"
          @click="clearReturned"
        >
          Visszahozott könyvek törlése
        </button>

      </div>


      <ul class="list-group list-group-flush">

        <BookItem
          v-for="book in sortedBooks"
          :key="book.id"
          :book="book"
          @remove="removeBook"
          @toggle-returned="toggleReturned"
        />

      </ul>

    </div>

  </div>
</template>