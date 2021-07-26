<template>
<div>
  <span v-for="(entrenador, index ) in Entrenadores" :key="entrenador.nombre">
  {{index}}<br>
  <span>Nombre</span><b-form-input class="w-25 h-25" v-model="entrenador.nombre" placeholder="Enter your name"></b-form-input>
  <span>Valoracion</span><input type="text" placeholder="app2">
  <span>Valoracion</span><input type="number" v-model="entrenador.plazas"><br>
  <a href="www.google.com" class="link-primary">Primary link</a>
  </span>

  <div>
    <b-button v-b-modal.modal-1>Ver tabla de clientes</b-button>
    <b-button v-b-modal="'my-modal'" v-on:click="isHidden = true">Calcular</b-button>
    <b-modal id="modal-1" title="Clientes" ok-only ok-variant="secondary" ok-title="Volver">
    <span v-for="(usuario, index) in Usuarios" :key="usuario.valor1">
    <span>{{index}} - {{usuario.importanciaReputacion}}</span><br>
    </span><br>
    </b-modal>
  </div>
</div>

</template>

<script>
  export default {
    data() {
      return {
        isHidden: false,
        "Entrenadores": {
        "Entrenador1":{"nombre":"A","reputacion":"4.5","plazas":1},
        "Entrenador2":{"nombre":"B","reputacion":"3.2","plazas":4},
        "Entrenador3":{"nombre":"C","reputacion":"1.2","plazas":3},
        "Entrenador4":{"nombre":"D","reputacion":"3.4","plazas":2}
        },
        "Usuarios": {
        "q":{"importanciaReputacion":2.6},
        "r":{"importanciaReputacion":3.7},
        "s":{"importanciaReputacion":8.5},
        "t":{"importanciaReputacion":9.7},
        "u":{"importanciaReputacion":2.6},
        "v":{"importanciaReputacion":4.7},
        "w":{"importanciaReputacion":5.6},
        "x":{"importanciaReputacion":3.7},
        "y":{"importanciaReputacion":8.1},
        "z":{"importanciaReputacion":2.5},
        } //
      }
      },

}
</script>
