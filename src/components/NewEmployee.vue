<template>
<div id="new-employee">
  <h3>
    Nuevo Empleado
  </h3>
  <div class="row">
    <form @submit.prevent="saveEmployee" class="col s12">
      <div class="input-field col s12">
        <input type="text" v-model="id_empleado" required>
        <label> ID de Empleado: </label>
      </div>
      <div class="input-field col s12">
        <input type="text" v-model="nombre" required>
        <label> Nombre: </label>
      </div>
      <div class="input-field col s12">
        <input type="text" v-model="area" required>
        <label> Area: </label>
      </div>
      <div class="input-field col s12 ">
        <input type="text" v-model="puesto" required>
        <label> Puesto: </label>
      </div>
      <button type="submit" class="btn green">Ingresar</button>
      <router-link to="/" class="btn grey">Cancelar</router-link>
    </form>
  </div>
</div>
</template>

<script>
import db from './firebaseInit'
  export default{
    name:'new-employee',
    data(){
      return{
        'id_empleado': null,
        'nombre': null,
        'area': null,
        'puesto': null
      }
    },
    methods: {
      saveEmployee(){
        db.collection('Empleados').add({
          id_empleado: this.id_empleado,
          nombre: this.nombre,
          area: this.area,
          puesto: this.puesto
        }).then(docRef => this.$router.push('/'))
          .catch(error => console.log(err))
      }
    }
  }

</script>
