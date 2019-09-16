<template>
<div id="dashboard">
  <ul class="collection with-header">
    <li class="collection-header">
      <h4>Empleados</h4>
    </li>
    <li v-for="employee in employees" :key="employee.id" class="collection-item">
      <div class="chip">
        {{employee.area}}
      </div>{{employee.id_empleado}}:{{employee.nombre}}

      <router-link class="secondary-content" v-bind:to="{name:'view-employee', params:{id_empleado: employee.id_empleado}}">
        <i class="fa fa-eye"></i>
      </router-link>
    </li>
  </ul>
  <div class="fixed-action-btn">
    <router-link to="/new" class="btn-floating btn-large red">
      <i class="fa fa-plus"></i>
    </router-link>
  </div>
</div>
</template>

<script>
import db from './firebaseInit'
export default {
  name: 'dashboard',
  data() {
    return {
      employees: []
    }
  },
  created() {
    db.collection('Empleados').orderBy('area').get().then(querySnapshot => {
      querySnapshot.forEach(doc => {
        const data = {
          'id': doc.id,
          'id_empleado': doc.data().id_empleado,
          'nombre': doc.data().nombre,
          'area': doc.data().area,
          'puesto': doc.data().puesto
        }
        this.employees.push(data)
      })
    })
  }

}
</script>
