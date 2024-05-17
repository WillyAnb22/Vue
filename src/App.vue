<template>
  <div class="for" style="text-align: center">
    <label for="name">Nombre </label>
    <input type="text" v-model="name" /><br /><br />
    <span v-if="errorName" class="error">{{ errorName }}</span>

    <label for="precio">Precio</label>
    <input type="text" v-model="precio" /><br /><br />
    <span v-if="errorPrecio" class="error">{{ errorPrecio }}</span>

    <label for="costo">Costo</label>
    <input type="text" v-model="costo" /><br /><br />
    <span v-if="errorCosto" class="error">{{ errorCosto }}</span>

    <label for="proveedor">Proveedor</label>
    <input type="text" v-model="prove" /><br /><br />
    <span v-if="errorProveedor" class="error">{{ errorProveedor }}</span>

    <button @click="guardar()">Agregar</button><br />

    <table>
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Precio</th>
          <th>Costo</th>
          <th>Cantidad</th>
          <th>Proveedor</th>
          <th>A</th>
          <th>D</th>
          <th>Ganancia</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in data" :key="index">
          <td>{{ item.nombre }}</td>
          <td>{{ item.precio }}</td>
          <td>{{ item.costo }}</td>
          <td>{{ item.cantidad }}</td>
          <td>{{ item.proveedor }}</td>
          <td>
            <button @click="aumentar(index)" v-if="item.cantidad < maxc">+</button>
          </td>
          <td>
            <button @click="disminuir(index)" v-if="item.cantidad > 0">-</button>
          </td>
          <td>{{ ganancia(item) }}</td>
        </tr>
      </tbody>
    </table>

    <label for="Ctotal">Costo total del inventario: {{ costoTotal }}</label><br>
    <label for="Ptotal">Precio total del inventario: {{ precioTotal }}</label><br>
    <label for="Gtotal">Ganancia total: {{ gananciaTotal }}</label>

  </div>
</template>

<script setup>
import { ref, computed } from "vue";

let name = ref("");
let precio = ref("");
let costo = ref("");
let prove = ref("");
let data = ref([]);
let maxc = 100;
let errorName = ref("");
let errorPrecio = ref("");
let errorCosto = ref("");
let errorProveedor = ref("");

function guardar() {
  // Validación de Nombre
  if (!name.value) {
    errorName.value = "Por favor, ingresa el Nombre.";
    setTimeout(() => {
      errorName.value = "";
    }, 3000);
    return;
  }

  // Validación de Precio
  if (!precio.value) {
    errorPrecio.value = "Por favor, ingresa el Precio.";
    setTimeout(() => {
      errorPrecio.value = "";
    }, 3000);
    return;
  }

  // Validación de Costo
  if (!costo.value) {
    errorCosto.value = "Por favor, ingresa el Costo.";
    setTimeout(() => {
      errorCosto.value = "";
    }, 3000);
    return;
  }

  // Validación de Proveedor
  if (!prove.value) {
    errorProveedor.value = "Por favor, ingresa el Proveedor.";
    setTimeout(() => {
      errorProveedor.value = "";
    }, 3000);
    return;
  }

  let d = {
    nombre: name.value,
    precio: precio.value,
    costo: costo.value,
    proveedor: prove.value,
    cantidad: 0,
  };

  data.value.push(d);

  // Limpiar los campos de entrada
  name.value = "";
  precio.value = "";
  costo.value = "";
  prove.value = "";
}

function aumentar(index) {
  if (data.value[index].cantidad < maxc) {
    data.value[index].cantidad++;
  }
}

function disminuir(index) {
  if (data.value[index].cantidad > 0) {
    data.value[index].cantidad--;
  }
}

function ganancia(item) {
  return (item.precio - item.costo)*(item.cantidad);
}

// Computed properties para calcular los totales
let costoTotal = computed(() => {
  return data.value.reduce((acc, item) => acc + (item.costo * item.cantidad), 0);
});

let precioTotal = computed(() => {
  return data.value.reduce((acc, item) => acc + (item.precio * item.cantidad), 0);
});

let gananciaTotal = computed(() => {
  return precioTotal.value - costoTotal.value;
});


</script>


<style>
table {
  width: 10%;
  right: 500px;
  box-shadow: 0 0 20px rgb(88, 87, 87);
  background-color: rgba(228, 227, 213, 0.748);
  margin-top: 10%;
  margin-left: 50px;
}
th,
td {
  width: 25%;
  text-align: left;
  vertical-align: top;
  border: 1px solid black;
  border-spacing: 0;
  padding: 0.3em;
}
.for {
  display: flex;
  flex-direction: column;
  padding: 10px;
  width: 49.5%;
  align-items: center;
  margin-left: 25%;
}
.error {
  color: red;
}
</style>
