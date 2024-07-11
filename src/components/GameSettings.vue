<template>
  <div>
    <h1
    >DIFFICULTY SETTINGS
    </h1>
    <button
      @click = "selectDifficulty('Easy')"
    > Easy
    </button>
    <br />
    <button
      @click = "selectDifficulty('Medium')"
    > Medium
    </button>
    <br />
    <button
      @click = "selectDifficulty('Hard')"
    > Hard
    </button>
  </div>
</template>

<script>
export default {
  name: 'GameSettings',
  methods: {
    selectDifficulty(difficulty){
      this.$emit('difficulty-selection', difficulty)
    }
  }
}
</script>
