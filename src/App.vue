<template>
  <div id="root">
    <GameSettings id="gameSettings"
      v-if="!difficultySetting"
      @difficulty-selection="(difficulty) => difficultySetting = difficulty"
    />
    <TheGame id="theGame"
      v-if="(!shownGame && difficultySetting)" msg="Welcome to Your Vue.js App"
      :difficultySetting="difficultySetting"
    />
  </div>
</template>

<script>
import TheGame from './components/TheGame.vue'
import GameSettings from './components/GameSettings.vue'

export default {
  name: 'App',
  components: {
    TheGame,
    GameSettings
  },
  data(){
    return{
      shownGame: 0,
      difficultySetting: null
    }
  }
}
</script>
