<template>
<div id="app">
 <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <div class="container-fluid">
        <router-link to="/" class="navbar-brand">Página Inicial</router-link>
        <div class="collapse navbar-collapse">
          <div class="navbar-nav">
            <router-link to="/liststudent" class="nav-link">Lista de Alunos</router-link>
            <router-link to="/addstudent" class="nav-link">Adicionar novo aluno</router-link>
          </div>
        </div>
      </div>
    </nav>
    <br/>
    <br/>
<div class="container">
<table class="table">
    <thead class="thead-dark">
      <tr>
        <th>Matrícula</th>
        <th>Nome</th>
        <th>Serie</th>
        <th>Escola</th>
        <th>Ações</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="student of students" :key="student.id">

        <td>{{student.id}}</td>
        <td>{{student.name}}</td>
        <td>{{student.serie}}</td>
        <td>{{student.school}}</td>
         <td>
              <router-link :to="{ path: '/editstudent/'+student.id}"><button  class="btn rounded-circle btn-success btn-sm"><i class="fas fa-pen"></i></button></router-link>
              <button v-on:click="deletar(student.id)" class="btn rounded-circle btn-danger btn-sm"><i class="fas fa-trash"></i></button>
            </td>
      </tr>
    </tbody>
  </table>
</div> 
</div>
</template>

<script>

import api from '@/services/api.js';

export default {
  data(){
    return {
      student: {
        id:'',
        name:'',
        serie:'',
        school:''

      }, 
      students: []
    }
  },
  mounted() {
    this.listar()

  },
  methods: {
    listar(){
        api.get('/students').then(response => {
        this.students = response.data;
    });
    },

    editar(id, data){
        api.put('/students/' + id, data).then(response => {
        this.students = response.data;
        this.listar()
    });
    },

    deletar(id){
        api.delete('/students/' + id).then(response => {
        this.students = response.data;
        alert("Aluno deletado com sucesso!")
        this.listar()
    });
    }
  }

}

</script>

<style>

</style>
