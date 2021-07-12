<template>
  <div id="status">
    <p class="font-serif text-3xl">{{ message }}{{ remaining }}</p>
  </div>
  <div class="content">
    <div class="square">
      <button
        class="item bg-green-700 border-2 border-black opacity-1 border-transparent-0 "
        v-on:click="Tap('green')"
        v-bind:class="{ bright: currentButton === 'green' }"
      ></button>
      <div class="item"></div>
      <button
        class="item bg-yellow-600 border-2 border-black opacity-1 border-transparent-0"
        v-on:click="Tap('orange')"
        v-bind:class="{ bright: currentButton === 'orange' }"
      ></button> 
      <div class="item">1</div>
      <button
        class="item bg-blue-500 border-2 border-black opacity-1 border-transparent-0"
        v-on:click="Tap('blue')"
        v-bind:class="{ bright: currentButton === 'blue' }"
      ></button>
      <div class="item">3</div>
      <button
        class="item bg-red-500 border-2 border-black opacity-1 border-transparent-0"
        v-on:click="Tap('red')"
        v-bind:class="{ bright: currentButton === 'red' }"
      ></button>
      <div class="item">2</div>
      <button
        class="item bg-purple-900 border-2 border-black opacity-1 border-transparent-0"
        v-on:click="Tap('purple')"
        v-bind:class="{ bright: currentButton === 'purple' }"
      ></button>
    </div>
    <div>
      <p><strong>Avance Actual: </strong>{{ current }}</p>
      <p><strong>Mayor Avance: </strong>{{ longest }}</p>
    </div>
    <button class="bg-blue-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded" v-on:click="start()">Start</button>
  </div>
</template>

<script>
export default {
  name: "Principal",
  data() {
    return {
      message: "Presiona el boton 'Start' para iniciar a jugar",
      remaining: "",
      intervalId: null,
      longest: 0,
      current: 0,
      sequence: [],
      taps: [],
      playSequenceId: null,
      currentButton: "",
      playSequenceCounter: 0,
      lights: ["red", "green", "orange", "blue", "purple"],
    };
  },
  methods: {
    //Iniciar juego
    start() {
      this.sequence = [];
      this.taps = [];
      this.isTimerActive = true;
      this.current = 0;
      this.playSequenceCounter = 0;
      this.playSequence();
    },
    //Pulsar botón cuando inicia secuencia
    Tap(color) {
        this.currentButton = color;
        var self = this;
        setTimeout(function () {
          self.currentButton = "";
        }, 300);
        var last_index = this.taps.length;
        this.taps.push(color);
        if (color === this.sequence[last_index]) {
          if (this.taps.length === this.sequence.length) {
            this.taps = [];
            this.current = this.sequence.length;
            if (this.longest < this.sequence.length) {
              this.longest = this.sequence.length;
            }
            setTimeout(function () {
              self.playSequence();
            }, 200);
          }
        } else {
          this.gameOver();
        }
      
    },
    //Agregar colores a la secuencia
    addToSequence() {
      var index = Math.floor(Math.random() * 5);
      this.sequence.push(this.lights[index]);
    },
    //Iniciar secuencia de colores
    playSequence() {
      var self = this;
      self.addToSequence();
      self.playSequenceId = window.setInterval(function () {
        self.currentButton = self.sequence[self.playSequenceCounter];
        setTimeout(function () {
          self.currentButton = "";
          self.playSequenceCounter++;
          if (self.playSequenceCounter === self.sequence.length) {
            window.clearInterval(self.playSequenceId);
            self.playSequenceCounter = 0;
          }
        }, 300);
      }, 600);
    },
    //Termina el juego
    gameOver() {
      //Mostrara mensaje cuando el jugador se equivoque, dara opción de volver a jugar
      this.$swal({
          title: 'Haz Perdido',
          text: '¿Quieres Volver a Jugar?',
          type: 'warning',
          showCancelButton: true,
          confirmButtonText: 'Reintentar',
          cancelButtonText: 'No',
          showCloseButton: true,
          showLoaderOnConfirm: true
        }).then((result) => {
          //Si el jugador pulsa "Reintentar", iniciara el juego en 0 y con el ultimo marcador
          if(result.value) {
            this.start();
          }  
          else {
            //De caso contrario, el juego mostrará el puntaje que hizo
            this.$swal('Tu Puntuación es de: '+ this.longest, 'Buen Intento' , 'info')
          }
        })
        this.current = 0;
    },
  },
};
</script>

<!--Estilos CSS-->
<style scoped>
* {
  text-align: center;
}

*:focus {
  outline: none !important;
}

.square {
  border: 2px solid #000;
  width: 500px;
  margin: auto;
  
  display: flex;
  flex-wrap:wrap;
  justify-content: space-between;
}

.item {
  color: #fff;
  flex: none;
  width: calc((100% - 0px) / 3);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 3em 0;
}

.bright {
  opacity: 0.3  !important;
}

</style>