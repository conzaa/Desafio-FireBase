<template>
  <div>
    <form @submit.prevent="addUsuarios">
      <input v-model="nuevoUsuarios" placeholder="Nuevo Usuarios" />
      <button type="submit">Agregar</button>
    </form>
    <ul>
      <li v-for="Usuario of Usuarios" :key="Usuario.id">
        {{ Usuario.id }} - {{ Usuario.nombre }} - {{ Usuario.correo }}
        <button @click="deleteUsuarios(Usuario.id)">Eliminar</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { getFirestore, collection, onSnapshot, addDoc, doc, deleteDoc } from 'firebase/firestore';
import App from '../config/firebase';

export default {

  data() {
    return {
      nuevoUsuarios: '',
      Usuarios: [],
    }
  },

  mounted() {
    const db = getFirestore(App);
    const UsuariosRef = collection(db, 'Usuarios');
    // "todos" es el nombre de la colección en Firestore
    onSnapshot(UsuariosRef, (snapshot) => {
      this.Usuarios = snapshot.docs.map((doc) => ({ id: doc.id, ...doc.data() }));
    });
  },
  methods: {
    async addUsuarios() {
      if (this.nuevoUsuarios.trim() === ''){
        alert("no has escrito nada")
        return;
      }

      if (this.nuevoUsuarios.length <3 ){
        alert(" mas de 3 caracteres")
        return;
      }
      
      const db = getFirestore(App);
      const UsuariosRef = collection(db, 'Usuarios');
      
      await addDoc(UsuariosRef, { nombre: this.nuevoUsuarios });
      this.nuevoUsuarios = ''; // Limpiar el campo después de agregar },

 
    },
       
    async deleteUsuarios(UsuariosId) {
      const db = getFirestore(App);
      const UsuariosRef = doc(db, 'Usuarios', UsuariosId);

      await deleteDoc(UsuariosRef);
    }
  }
};
</script>

<style></style>
