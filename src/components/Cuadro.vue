<template>
  <div class="cuadro" v-on:click="changeValor">
    <div class="valor_cont x_cont" v-if="valor === 'X'">
      X
    </div>
    <div class="valor_cont o_cont" v-else-if="valor === 'O'">
      O
    </div>
    <div class="valor_cont" v-else>
      {{ valor }}
    </div>
  </div>
</template>

<script>
export default {
  props: ['posf', 'posc', 'valor'],
  methods: {
    changeValor () {
      this.$parent.marcarCuadro(this.posf, this.posc)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.cuadro {
  width: 50px;
  height: 50px;
  text-align: center;
}

.valor_cont {
  font-size: 40px;
  color: #222222;
}

.x_cont {
  color: #AA2222;
}

.o_cont {
  color: #22AA22;
}
</style>

