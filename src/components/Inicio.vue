<template>
  <b-container>
    <b-row v-for="(filaCuadros, filaIdx) in cuadros" :key="filaIdx">
      <Cuadro v-for="(cuadro, colIdx) in filaCuadros"
          :key="filaIdx + '_' + colIdx" :posf="filaIdx" :posc="colIdx" :valor="cuadro">
      </Cuadro>
    </b-row>
    <b-row><b-col>&nbsp;</b-col></b-row>
    <b-row>
      <b-col>
        <div v-if="alguienGano">
          <b-alert show variant="success">
            Gano {{ ganador }}
          </b-alert>
          <br/>
          <b-button variant="primary" v-on:click="reiniciarJuego">Volver a iniciar</b-button>
        </div>
        <div v-else>
          <b-alert show variant="warning">
            Le toca a {{ estadoActual }}
          </b-alert>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import Cuadro from '@/components/Cuadro'

export default {
  name: 'Holas',
  components: {Cuadro: Cuadro},
  data () {
    return {
      cuadros: [],
      estadoActual: 'X',
      ganador: '-',
      alguienGano: false
    }
  },
  methods: {
    reiniciarJuego () {
      for (let i in this.cuadros) {
        for (let j in this.cuadros[i]) {
          this.cuadros[i][j] = '-'
        }
      }

      this.estadoActual = 'X'
      this.ganador = '-'
      this.alguienGano = false
    },
    chequearGanador () {
      // Filas
      for (let i = 0; i < 3; i++) {
        if ((this.cuadros[i][0] === 'X' || this.cuadros[i][0] === 'O') &&
          this.cuadros[i][0] === this.cuadros[i][1] && this.cuadros[i][2] === this.cuadros[i][1]) {
          this.ganador = this.cuadros[i][0]
          this.alguienGano = true
          return
        }
      }

      // Columnas
      for (let j = 0; j < 3; j++) {
        if ((this.cuadros[0][j] === 'X' || this.cuadros[0][j] === 'O') &&
          this.cuadros[0][j] === this.cuadros[1][j] && this.cuadros[2][j] === this.cuadros[1][j]) {
          this.ganador = this.cuadros[0][j]
          this.alguienGano = true
          return
        }
      }

      // Diagonales
      if (this.cuadros[1][1] === 'X' || this.cuadros[1][1] === 'O') {
        // Diagonal primaria
        if (this.cuadros[0][0] === this.cuadros[1][1] && this.cuadros[0][0] === this.cuadros[2][2]) {
          this.ganador = this.cuadros[1][1]
          this.alguienGano = true
          return
        }

        // Diagonal secundaria
        if (this.cuadros[0][2] === this.cuadros[1][1] && this.cuadros[1][1] === this.cuadros[2][0]) {
          this.ganador = this.cuadros[1][1]
          this.alguienGano = true
        }
      }
    },
    marcarCuadro (posf, posc) {
      let valorAnterior = this.cuadros[posf][posc]
      // let valorNuevo = valorAnterior

      if (!this.alguienGano && valorAnterior !== 'X' && valorAnterior !== 'O') {
        this.cuadros[posf][posc] = this.estadoActual
        // valorNuevo = this.estadoActual
        if (this.estadoActual === 'X') {
          this.estadoActual = 'O'
        } else {
          this.estadoActual = 'X'
        }
        this.chequearGanador()
      }

      // return valorNuevo
    },
    getValorCuadro (posf, posc) {
      return this.cuadros[posf][posc]
    }
  },
  mounted () {
    for (let i = 0; i < 3; i++) {
      let filaCuadros = []
      for (let j = 0; j < 3; j++) {
        filaCuadros.push('-')
      }
      this.cuadros.push(filaCuadros)
    }

    this.reiniciarJuego()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
