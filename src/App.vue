<template>
<div>
<!-- pag resultados -->
<div v-if="buttonClicked" ><!-- if que muestra pag al pulsar boton -->
<!-- tabs que muestra la pagina de entrenadores y la pagina donde se explica los resultados -->
<ul class="nav nav-tabs mt-5" role="tablist">
  <li class="nav-item ">
    <a class="nav-link active" href="#entrenadores" role="tab" data-toggle="tab">Entrenadores</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#resultados" role="tab" data-toggle="tab">Resultados</a>
  </li>
</ul>


<div class="tab-content">
  <div role="tabpanel" class="tab-pane active border pt-1 pb-2 mb-5" id="entrenadores">
  <!-- container que muestra el entrenador y su diversa informacion -->
    <div class="row mr-1 ml-5 mb-3 w-75 h-100"><!-- fila cabecera -->
    <!-- fila cabecera -->
      <div class="col ml-3  w-50 mb-5 mt-5" v-for="(entrenador,index) in Resultado" :key="entrenador">
      <div class="row bg-info rounded-top pb-2 pt-2">
      <div class="col " ><div class="float-left">Entrenador {{sumar(index)}}</div></div>
      <div class="col  float-right"><div class="float-right"><img src="./assets/info.svg" width="30" height="30" class=" float-right rounded-circle align-bottom" :id="'but'+index"></div></div>
<!-- fila cabecera -->
    </div>
    <div class="row border-right border-left h-75"><!-- fila body -->
<!-- fila cabecera -->
      <div class="col col-4 pl-1"><img src="./assets/logo.png" class="rounded-circle" width="40" height="40"></div>
      <div class="col ">
      <h5 class="row float-center">{{entrenador.nombre}}</h5>
      <div class="row float-center" >Clientes asignados:</div>
      <div v-for="(usuario,index) in entrenador.usuarios" :key="usuario" >
      <div class="row">-Cliente: {{entrenador.usuarios[index].nombre}}</div>
      </div>
      </div>
<!-- fila cabecera -->
    </div>
    <div class="row bg-info mb-5 pl-2 pt-3 pb-3 rounded-bottom"><!-- fila footer -->
      <div v-if="entrenador.cantidadUsuarios.valoracion">Clientes asignados: {{entrenador.cantidadUsuarios.valoracion}}</div>
      <div v-else>Clientes asignados: 0</div>
    </div>
    </div>
  </div>
</div>

<b-tooltip target="but0" placement="bottom"><!-- tooltip hover1 -->
<strong>Valoracion: </strong>{{Resultado[0].valorFinal.valorFinal}}
<br>
<strong>Max clientes: </strong>{{Resultado[0].plazas}}
</b-tooltip>

<b-tooltip target="but1" placement="bottom"><!-- tooltip hover2 -->
<strong>Valoracion: </strong>{{Resultado[1].valorFinal.valorFinal}}
<br>
<strong>Max clientes: </strong>{{Resultado[1].plazas}}
</b-tooltip>

<b-tooltip target="but2" placement="bottom"><!-- tooltip hover3 -->
<strong>Valoracion: </strong>{{Resultado[2].valorFinal.valorFinal}}
<br>
<strong>Max clientes: </strong>{{Resultado[2].plazas}}
</b-tooltip>

<b-tooltip target="but3" placement="bottom"><!-- tooltip hover4 -->
<strong>Valoracion: </strong>{{Resultado[3].valorFinal.valorFinal}}
<br>
<strong>Max clientes: </strong>{{Resultado[3].plazas}}
</b-tooltip>


<div role="tabpanel" class="tab-pane border pl-1 pr-4" id="resultados"><!-- tab explicacion resultados -->
Se ha usado una formula que nos permita lograr calcular un indice de satisfaccion entre la reputación de cada entrenador y la importancia que estos le dan los usuarios, teniendo en cuenta los siguientes principios:
<br>
-1: un usuario que da una importancia mayor a 5 a la reputacion del entrenador va a valorar de forma mas positiva que el entrenador tenga mucha reputacion y de forma negativa poca reputacion
<br>
(ej:un usuario de reputacion mayor a 5 debe obtener una satisfaccion alta con un reputado entrenador y una satisfaccion baja con un entrenador poco reputado)
<br>
-2: un usuario que da una importancia menor a 5 a la reputacion del entrenador obtiene un indice de satisfaccion independiente de la reputacion, en el cual mientras menos importancia de, mas satisfaccion se produce
<br>
(ej:un usuario que de 0 importancia va a obtener una satisfaccion 10 independientemente del entrenador, a menos importancia mas valoracion independiente de la reputacion)
<br>
con lo cual la formula usada ha sido:
<br>
<br>
a=reputacion entrenador
<br>
b=importancia reputacion usuario
<br>
si es mayor o igual a 5: (b/10)*(a*2)/10
<br>
si es menor a 5: ((10-b)/10)*(a*2)
<br>
Notas adicionales:(En caso de n plazas, se añade el paso de sumar todas las importancias y dividirlas entre la cantidad de importancias)<br>

<div v-for="entrenador in Resultado" :key="entrenador">

<br>
Entrenador: {{entrenador.nombre}}<br>
Reputacion: {{entrenador.reputacion}}<br>
    valoracion media: {{entrenador.valores.valoracion}}<br>
    <div v-for="usuario in entrenador.usuarios" :key="usuario">
    valoracion individual: {{usuario.nombre}} - {{usuario.importanciaReputacion}}<br>
    </div>
    valoracion final: {{entrenador.valorFinal.valorFinal}}<br>
</div>
<br>
</div>
</div>
<b-button class="btn btn-success float-right mr-5 bg-white btn btn-outline-secondary" v-b-modal="'my-modal'" @click="borrarDatos">Volver</b-button><!-- boton vuelta a inicio -->
</div>

<!-- formulario inicial -->
  <div v-if="buttonClicked2" class="container-fluid">
    <div class="mb-2" v-for="(entrenador, index ) in Entrenadores" :key="entrenador.nombre">
    <h5>{{index}}</h5><!-- cabecera -->
  <form>
    <div class="form-row"><!-- fila -->
    <label class="col-form-label mt-3">Nombre</label><!-- input nombre -->
      <div class="col">
      <input id="pequeño" type="text" class="form-control mt-3" v-model.trim.lazy="entrenador.nombre" placeholder="Introducir nombre">
      </div>
    <label class="col-form-label mt-3">Reputacion</label><!-- input reputacion -->
      <div class="col">
      <input type="text" class="form-control mt-3" v-model="entrenador.reputacion">
      </div>
    <label class="col-form-label mt-3">Numero maximo de clientes</label>
      <div class="col">
      <input type="number" class="form-control mt-3" v-model="entrenador.plazas"><!-- input plazas -->
      </div>
    </div><!--fin fila -->
  </form><!--fin form -->
  <div class="border-bottom border-primary pb-4 ml-3 mr-3"></div><!-- borde separador -->
    </div>
    <div class="float-right mb-5">
  <br>

<!-- BOTONES -->
<b-button v-b-modal.modal-1 class="bg-white btn btn-outline-secondary">Ver tabla de clientes</b-button><!-- boton mostrar modal clientes -->
<b-button class="btn btn-success ml-5" v-b-modal="'my-modal'" @click="loadTheTemplateAbove">Calcular</b-button><!-- boton mostrar pagina resultados -->
<!--modal clientes -->
    <b-modal id="modal-1" title="Clientes" ok-only ok-variant="secondary" ok-title="Volver">
    <span v-for="(usuario, index) in Usuarios" :key="usuario.valor1">
    <span>{{index}} - {{usuario.importanciaReputacion}}</span><br>
    </span><br>
    </b-modal>
<!--modal clientes -->
  </div>
  </div>
</div>


</template>

<script>
  export default {
    data() {
      return {
        buttonClicked: false,
        buttonClicked2: true,
        //datos entrenadores
        "Entrenadores": {
        "Entrenador 1":{"nombre":"A","reputacion":"4.5","plazas":1},
        "Entrenador 2":{"nombre":"B","reputacion":"3.2","plazas":4},
        "Entrenador 3":{"nombre":"C","reputacion":"1.2","plazas":3},
        "Entrenador 4":{"nombre":"D","reputacion":"3.4","plazas":2}
        },
        //datos usuario
        "Usuarios": {
        "0":{"nombre":"q","importanciaReputacion":2.6},
        "1":{"nombre":"r","importanciaReputacion":3.7},
        "2":{"nombre":"s","importanciaReputacion":8.5},
        "3":{"nombre":"t","importanciaReputacion":9.7},
        "4":{"nombre":"u","importanciaReputacion":2.6},
        "5":{"nombre":"v","importanciaReputacion":4.7},
        "6":{"nombre":"w","importanciaReputacion":5.6},
        "7":{"nombre":"x","importanciaReputacion":3.7},
        "8":{"nombre":"y","importanciaReputacion":8.1},
        "9":{"nombre":"z","importanciaReputacion":2.5},
        },
        //datos donde guardamos los resultados calculados
        "Resultado":{} //
      }
      },
      methods: {
      //RESULTADOS//////////////////////////////
  loadTheTemplateAbove() {
    ///////////////////////VARIABLES A UTILIZAR/////////////////////////////////////////////////////////////////////////////////
    this.buttonClicked = !this.buttonClicked;
    this.buttonClicked2 = !this.buttonClicked2;
    let i;
    let x;
    let y=0;
    let suma=0;
    let cantidadUsuarios=0;
    let lenUsuarios=Object.keys(this.Usuarios).length;
    let lenEntrenador=Object.keys(this.Entrenadores).length;

    ////////////////devuelve usuarios y entrenadores ordenados por la reputacion de mayor a menor/////////////////////////////
    let usuarioSort=Object.entries(this.Usuarios).sort((a,b) => b[1].importanciaReputacion-a[1].importanciaReputacion);
    let entrenadorSort=Object.entries(this.Entrenadores).sort((a,b) => b[1].reputacion-a[1].reputacion);
    /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

    let nombre=Object.values(this.Entrenadores);
    /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


//bucle que recorre los entrenadores y los guarda
for(i=0;i<=lenEntrenador;i++){
///////////////////////guardamos los entrenadores ordenados
this.Resultado.[i] = Object.assign({},this.Resultado.[i], {"nombre":entrenadorSort[i][1].nombre,"reputacion":entrenadorSort[i][1].reputacion,"plazas":entrenadorSort[i][1].plazas});

/////////////////////guardamos los campos valorFinal, valores y cantidadUsuarios necesarios para la pagina resultados
this.Resultado.[i].valorFinal = Object.assign({},this.Resultado.[i].valorFinal, {});
this.Resultado.[i].valores = Object.assign({},this.Resultado.[i].valores, {});
this.Resultado.[i].cantidadUsuarios = Object.assign({},this.Resultado.[i].cantidadUsuarios, {});


//bucle que recorre todos los usuarios de cada entrenador y los almacena
for(x=0;x<lenUsuarios;x++){

//condicional para que solo guarde los usuarios correspondientes a las plazas maximas recibidas
if(x<entrenadorSort[i][1].plazas && y<10){
cantidadUsuarios++;
this.Resultado.[i].usuarios = Object.assign({},this.Resultado.[i].usuarios, {});
this.Resultado.[i].usuarios[y] = Object.assign({},this.Resultado.[i].usuarios[y], usuarioSort[y][1]);

///////sumamos la reputacion de cada usuario
suma+=usuarioSort[y][1].importanciaReputacion;
y++;
}
////guardamos la cantidad de usuarios que han sido asignados a cada entrenador
this.Resultado.[i].cantidadUsuarios = Object.assign({},this.Resultado.[i].cantidadUsuarios, {"valoracion":cantidadUsuarios});

/////condicional para guardar la valoracion media y final, solo debe entrar una vez
if(x+1==nombre[i].plazas){
///////////condicional suma, en caso de que no haya usuarios asignados se guarda como 0, si no normal, evita division por 0
if(suma!=0){
suma=suma/cantidadUsuarios;
}else{
suma=0;
}

//guardamos valoracion media
this.Resultado.[i].valores = Object.assign({},this.Resultado.[i].valores, {"valoracion":suma});
/////////condicional
if(suma<=5){
suma=10-suma;
suma=suma/10;
let suma2=10-entrenadorSort[i][1].reputacion*2;
let suma3=suma*suma2;
///////////guardamos valoracion final <5
this.Resultado.[i].valorFinal = Object.assign({},this.Resultado.[i].valorFinal, {"valorFinal":suma3});
}
else{
suma=suma/10;
let suma2=entrenadorSort[i][1].reputacion*2;
let suma3=suma*suma2;
///////////guardamos valoracion final>5
this.Resultado.[i].valorFinal = Object.assign({},this.Resultado.[i].valorFinal, {"valorFinal":suma3});
    }//finaliza condicional guardado valor medio y final
  }//finaliza guardado
}//finaliza bucle usuario de entrenador
cantidadUsuarios=0;
suma=0;
}//finaliza entrenador
},
  borrarDatos(){
  this.buttonClicked = !this.buttonClicked;
  this.buttonClicked2 = !this.buttonClicked2;
  this.Resultado = {};
  },
  sumar(index){
  let suma = parseInt(index);
  suma++;
  return suma;
  }
}////////////FIN METODOS////////////////////////////////////
}
</script>

<style>
</style>
