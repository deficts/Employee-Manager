<template>
<div id="view-employee">
  <ul class="collection with-header">
    <li class="collection-header">
      <h4>
        {{nombre}}
      </h4>
    </li>
    <li class="collection-item">
      Employee ID: {{id_empleado}}
    </li>
    <li class="collection-item">
      Area: {{area}}
    </li>
    <li class="collection-item">
      Puesto: {{puesto}}
    </li>
  </ul>
  <router-link to="/" class="btn grey">Regresar</router-link>
  <button @click="deleteEmployee" class="btn red">Borrar</button>
  <div class="fixed-action-btn">
    <router-link :to="{name:'edit-employee', params:{id_empleado: id_empleado}}" class="btn-floating btn-large red">
      <i class="fa fa-pencil green"></i>
    </router-link>
  </div>
</div>
</template>

<script>
import db from './firebaseInit'
export default {
  name: 'view-employee',
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
    deleteEmployee() {
      if (confirm('¿Seguro que quieres borrar este elemento?')) {
        db.collection('Empleados').where('id_empleado', '==', this.$route.params.id_empleado).get()
          .then(querySnapshot => {
              querySnapshot.forEach(doc => {
                  doc.ref.delete()
                  this.$router.push('/')
                  })
              })
          }
      }
    }
  }
</script>
