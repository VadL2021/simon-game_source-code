<template>
  <div>
    <h1>Difficulty: {{ difficultySetting }}</h1>
    <div class="gameTablet">
      <div class="yellow"
        @click="currentButton = changeCurrentButtonTo('yellow')"
        :class="{active: isYellowActive}"
      > 
      </div>
      <div class="blue" 
        @click="currentButton = changeCurrentButtonTo('blue')"
        :class="{active: isBlueActive}"
      > 
      </div>
      <div class="green"
        @click="currentButton = changeCurrentButtonTo('green')"
        :class="{active: isGreenActive}"
      > 
      </div>
      <div class="red"
        @click="currentButton = changeCurrentButtonTo('red')"
        :class="{active: isRedActive}"
      > 
      </div>
    </div>
    <button
      @click="generateNextElementInGameArray()"
      v-if="!gameStage"
    > start game 
    </button>
    <h1
      v-if="gameStage"
    > current stage: {{ gameStage }} 
    </h1>
  </div>
</template>

<script>
import do_mp3 from '../assets/do.mp3'
import re_mp3 from '../assets/re.mp3'
import mi_mp3 from '../assets/mi.mp3'
import sol_mp3 from '../assets/sol.mp3'
export default {
  name: 'TheGame',
  props: {
    difficultySetting:{
      type: Number(String)
    }
  },
  data(){
    return {
      orderInGameArray: [],
      orderInPlayerArray: [],
      currentButton: null,
      gameStage: null,

      isYellowActive: false, 
      isBlueActive: false, 
      isGreenActive: false, 
      isRedActive: false, 

      buttonsLocked: true
    }
  },
  watch:{
    currentButton(clickedColoredButton){
      if (clickedColoredButton){
        this.playSoundBindedToColor(clickedColoredButton)

        let playerArrayLength = this.orderInPlayerArray.length
        let gameArrayLength = this.orderInGameArray.length

        if (playerArrayLength < gameArrayLength){
          this.orderInPlayerArray.push(clickedColoredButton)
          playerArrayLength = this.orderInPlayerArray.length
          let newElementIndex = playerArrayLength-1
          if(this.orderInPlayerArray[newElementIndex] != this.orderInGameArray[newElementIndex]){
            this.stopGame()
          }
          if(String(this.orderInPlayerArray) == String(this.orderInGameArray) && this.orderInGameArray.length != 0){
            this.startNextGameStage()
          }
        } else {
          console.log('Game did not start')
        }
        this.currentButton = null
      }
    },
    orderInGameArray(newOrder){
      this.gameStage = newOrder.length
      let pauseBetweenSounds
      switch(this.difficultySetting){
        case 'Easy':
          pauseBetweenSounds = 1500
          break
        case 'Medium':
          pauseBetweenSounds = 1000
          break
        case 'Hard':
          pauseBetweenSounds = 400
          break
      }
      //Задержка перед звуковым сигналом (0.5 секунд)
      let soundIndexInGameArray = 0
      //Индекс элемента, нужный для
      //создания промежутков между 
      //звуковыми сигналами
      let newStagePauseModifier = 1000
      //Модификатор для создания паузы 
      //в начале нового раунда (1 секунда)
      newOrder.forEach((color,)=>{
        setTimeout(()=>{
          this.playSoundBindedToColor(color)
        }, newStagePauseModifier + soundIndexInGameArray * pauseBetweenSounds)
        soundIndexInGameArray += 1
      })
      setTimeout(()=>{//Разблокирование кнопок после того как 
        //будет создана новая последовательность в массиве игры
        this.buttonsLocked = false
      }, soundIndexInGameArray * pauseBetweenSounds)
    }
  },
  methods:{
    generateNextElementInGameArray(){ 
      //Сгенерировать следующий случайный цвет
      let randomNubmerToSelectColor = this.getRandomInt(1, 4)
      switch(randomNubmerToSelectColor){
        case 1: 
          this.orderInGameArray.push('yellow')
          break
        case 2: 
          this.orderInGameArray.push('blue')
          break
        case 3: 
          this.orderInGameArray.push('green')
          break
        case 4:  
          this.orderInGameArray.push('red')
          break
      }
    },
    getRandomInt(min, max) {
      //Сгенерировать случайное число 
      //от min до max включительно
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    playSoundBindedToColor(color){
      if (color){
        let audio 
        let keyShadingDuration = 200
        switch(color){
          case 'yellow':
            audio = new Audio(do_mp3)
            this.isYellowActive = true
            setTimeout(()=>{
              this.isYellowActive = false
            }, keyShadingDuration)
            break
          case 'blue':
            audio = new Audio(re_mp3)
            this.isBlueActive = true
            setTimeout(()=>{
              this.isBlueActive = false
            }, keyShadingDuration)
            break
          case 'green':
            audio = new Audio(mi_mp3)
            this.isGreenActive = true
            setTimeout(()=>{
              this.isGreenActive = false
            }, keyShadingDuration)
            break
          case 'red':
            audio = new Audio(sol_mp3)
            this.isRedActive = true
            setTimeout(()=>{
              this.isRedActive = false
            }, keyShadingDuration)
            break
        }
        audio.play()
      } else{
        console.log('color added with error - no sound will be played')
      }
    },
    startNextGameStage(){
    //Первый этап игры также включается
    //этим методом
      this.buttonsLocked = true
      this.generateNextElementInGameArray()
      this.orderInPlayerArray = []
    },
    stopGame(){
      this.buttonsLocked = true
      this.orderInPlayerArray = []
      this.orderInGameArray = []
      alert('You lost!')
    },
    changeCurrentButtonTo(color){
      if (!this.buttonsLocked) return color
    }
  }
}
</script>