<template>
  <div>
    <nav class="navbar navbar-dark bg-dark">
      <div class="container">
        <span class="navbar-brand mb-0 h1">Score Tracker</span>
      </div>
    </nav>

    <div class="container my-4">
      <div class="card mb-3">
        <div class="card-body">
          <h5 class="card-title mb-3">Új játékos</h5>

          <form class="row g-2 align-items-end" @submit.prevent="addPlayer">
            <div class="col-12 col-md-6">
              <label class="form-label">Név</label>
              <input
                v-model.trim="newName"
                type="text"
                class="form-control"
                placeholder="Pl. Anna"
              />
            </div>

            <div class="col-12 col-md-3">
              <label class="form-label">Lépésköz (step)</label>
              <input
                v-model="stepInput"
                type="number"
                min="1"
                step="1"
                class="form-control"
                placeholder="1"
              />
            </div>

            <div class="col-12 col-md-3 d-grid">
              <button class="btn btn-primary" type="submit">
                Hozzáadás
              </button>
            </div>
          </form>

          <div v-if="error" class="alert alert-danger mt-3 mb-0">
            {{ error }}
          </div>
        </div>
      </div>

      <div class="card mb-3">
        <div class="card-body">
          <div class="d-flex flex-wrap justify-content-between align-items-center gap-2">
            <h5 class="card-title mb-0">Statisztika</h5>
            <button
              class="btn btn-outline-secondary btn-sm"
              @click="resetScores"
              :disabled="players.length === 0"
            >
              Pontok nullázása
            </button>
          </div>

          <div class="row mt-3 g-3">
            <div class="col-6 col-lg-2">
              <div class="small text-muted">Játékosok</div>
              <div class="fw-bold">{{ totalPlayers }}</div>
            </div>

            <div class="col-6 col-lg-2">
              <div class="small text-muted">Legjobb pont</div>
              <div class="fw-bold">
                {{ totalPlayers === 0 ? "-" : topScore }}
              </div>
            </div>

            <div class="col-12 col-lg-3">
              <div class="small text-muted">Vezető</div>
              <div class="fw-bold">
                {{ totalPlayers === 0 ? "-" : leaderName }}
              </div>
            </div>

            <div class="col-6 col-lg-2">
              <div class="small text-muted">Győztesek</div>
              <div class="fw-bold">{{ winnersCount }}</div>
            </div>

            <div class="col-6 col-lg-3">
              <div class="small text-muted">Átlagpont</div>
              <div class="fw-bold">
                {{ totalPlayers === 0 ? "-" : averageScore.toFixed(2) }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="card">
        <div class="card-body">
          <h5 class="card-title mb-3">Játékosok</h5>

          <div v-if="players.length === 0" class="text-muted">
            Nincs még játékos. Adj hozzá az űrlappal!
          </div>

          <ul v-else class="list-group">
            <PlayerRow
              v-for="p in players"
              :key="p.id"
              :player="p"
              @remove="removePlayer"
              @change-score="changeScore"
              @toggle-winner="toggleWinner"
            />
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PlayerRow from "./components/PlayerRow.vue";

export default {
  name: "App",
  components: { PlayerRow },

  data() {
    return {
      players: [
        { id: 1, name: "Anna", score: 0, winner: false },
        { id: 2, name: "Béla", score: 5, winner: true },
      ],
      newName: "",
      error: "",
      step: 1,

      stepInput: 1,

      nextId: 3,
    };
  },

  computed: {
    totalPlayers() {
      return this.players.length;
    },

    topScore() {
      if (this.players.length === 0) return 0;
      return Math.max(...this.players.map((p) => p.score));
    },

    leaderName() {
      if (this.players.length === 0) return "";
      const best = this.topScore;
      const leaders = this.players.filter((p) => p.score === best);
      if (leaders.length === 1) return leaders[0].name;
      return "Többen vezetnek";
    },

    winnersCount() {
      return this.players.filter((p) => p.winner).length;
    },

    averageScore() {
      if (this.players.length === 0) return 0;
      const sum = this.players.reduce((acc, p) => acc + p.score, 0);
      return sum / this.players.length;
    },
  },

  methods: {
    validateStep() {
      const n = Number(this.stepInput);

      if (!Number.isInteger(n) || n <= 0) {
        this.error = "A step csak pozitív egész szám lehet.";
        return false;
      }

      this.step = n;
      return true;
    },

    addPlayer() {
      this.error = "";

      if (!this.validateStep()) return;

      const name = this.newName.trim();
      if (!name) {
        this.error = "A játékos neve nem lehet üres.";
        return;
      }

      const exists = this.players.some(
        (p) => p.name.toLowerCase() === name.toLowerCase()
      );
      if (exists) {
        this.error = "Már létezik ilyen nevű játékos.";
        return;
      }

      this.players.push({
        id: this.nextId++,
        name,
        score: 0,
        winner: false,
      });

      this.newName = "";
    },

    removePlayer(id) {
      this.players = this.players.filter((p) => p.id !== id);
    },

    changeScore({ id, delta }) {
      const p = this.players.find((x) => x.id === id);
      if (!p) return;

      p.score += delta;

      if (p.score < 0) p.score = 0;
    },

    toggleWinner(id) {
      const p = this.players.find((x) => x.id === id);
      if (!p) return;
      p.winner = !p.winner;
    },

    resetScores() {
      this.players.forEach((p) => (p.score = 0));
    },
  },
};
</script>