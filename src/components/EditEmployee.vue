<template>
<div id="edit-employee">
  <h3>
    Edit Employee
  </h3>
  <div class="row">
    <form @submit.prevent="updateEmployee" class="col s12">
      <div class="input-field col s12">
        <input disabled type="text" v-model="id_empleado" required>
        <label class="active"> ID Empleado: </label>
      </div>
      <div class="input-field col s12">
        <input type="text" v-model="nombre" required>
        <label class="active"> Nombre: </label>
      </div>
      <div class="input-field col s12">
        <input type="text" v-model="area" required>
        <label class="active"> Area: </label>
      </div>
      <div class="input-field col s12 ">
        <input type="text" v-model="puesto" required>
        <label class="active"> Puesto: </label>
      </div>
      <button type="submit" class="btn green">Actualizar</button>
      <router-link :to="{name:'view-employee', params:{id_empleado: id_empleado}}" class="btn grey">Cancelar</router-link>
    </form>
  </div>
</div>
</template>

<script>
import db from './firebaseInit'
export default {
  name: 'edit-employee',
  data() {
    return {
      'id_empleado': null,
      'nombre': null,
      'area': null,
      'puesto': null
    }
  },
  beforeRouteEnter(to, from, next) {
    db.collection('Empleados').where('id_empleado', '==', to.params.id_empleado)
      .get().then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.id_empleado = doc.data().id_empleado
            vm.nombre = doc.data().nombre
            vm.area = doc.data().area
            vm.puesto = doc.data().puesto
          })
        })
      })
  },
  watch: {
    '$route': 'fetchData'
  },
  methods: {
    fetchData() {
      db.collection('Empleados').where('id_empleado', '==', this.$route.params.id_empleado).get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            this.id_empleado = doc.data().id_empleado
            this.nombre = doc.data().nombre
            this.area = doc.data().area
            this.puesto = doc.data().puesto
          })
        })
    },
    updateEmployee() {
      db.collection('Empleados').where('id_empleado', '==', this.$route.params.id_empleado).get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            doc.ref.update({
              id_empleado: this.id_empleado,
              nombre: this.nombre,
              area: this.area,
              puesto: this.puesto
            }).then(()=>{
              this.$router.push({name: 'view-employee', params: {id_empleado: this.id_empleado}})
            })
          })
        })
    }
  }
}
</script>
