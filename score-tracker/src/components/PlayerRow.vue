<template>
  <li
    class="list-group-item d-flex justify-content-between align-items-center"
    :class="{ 'list-group-item-success': player.winner }"
  >
    <div class="me-3">
      <div class="d-flex align-items-center gap-2">
        <strong>{{ player.name }}</strong>

        <span v-if="player.winner" class="badge text-bg-success">
          Győztes
        </span>
      </div>

      <div class="text-muted small">
        Pontszám: <span class="fw-semibold">{{ player.score }}</span>
      </div>
    </div>

    <div class="btn-group" role="group" aria-label="Player actions">
      <button class="btn btn-outline-success btn-sm" @click="onInc" title="+ pont">
        <i class="bi bi-plus-circle"></i>
      </button>

      <button class="btn btn-outline-warning btn-sm" @click="onDec" title="- pont">
        <i class="bi bi-dash-circle"></i>
      </button>

      <button
        class="btn btn-outline-primary btn-sm"
        @click="onToggleWinner"
        title="Győztes váltás"
      >
        <i
          class="bi"
          :class="player.winner ? 'bi-trophy-fill' : 'bi-trophy'"
        ></i>
      </button>

      <button class="btn btn-outline-danger btn-sm" @click="onRemove" title="Törlés">
        <i class="bi bi-trash"></i>
      </button>
    </div>
  </li>
</template>

<script>
export default {
  name: "PlayerRow",

  props: {
    player: {
      type: Object,
      required: true,
    },
  },

  emits: ["remove", "change-score", "toggle-winner"],

  methods: {
    onInc() {
      this.$emit("change-score", { id: this.player.id, delta: +1 });
    },

    onDec() {
      this.$emit("change-score", { id: this.player.id, delta: -1 });
    },

    onToggleWinner() {
      this.$emit("toggle-winner", this.player.id);
    },

    onRemove() {
      this.$emit("remove", this.player.id);
    },
  },
};
</script>