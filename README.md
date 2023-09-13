<template>
    <div>
      <h1>Tienda</h1>
      <form @submit.prevent="agregarArticulo">
      <!-- Campos del formulario -->
      <div>
        <label for="nombre">Nombre del artículo:</label>
        <input v-model="nombre" type="text" id="nombre" placeholder="Ingrese el nombre del artículo" required>
      </div>
      <div>
        <label for="cantidad">Cantidad:</label>
        <input v-model.number="cantidad" type="number" id="cantidad" placeholder="Ingrese la cantidad" required>
      </div>
      <div>
        <label for="valor">Valor unitario:</label>
        <input v-model.number="valor" type="number" id="valor" placeholder="Ingrese el valor unitario en pesos $" required>
      </div>
      <button type="submit">Agregar</button>
    </form>
      <table>
        <!-- Encabezado de la tabla -->
        <thead>
          <tr>
            <th>Identificador</th>
            <th>Nombre del artículo</th>
            <th>Cantidad</th>
            <th>Valor unitario</th>
            <th>Valor total</th>
            <th>Eliminar</th>
          </tr>
        </thead>
        <!-- Cuerpo de la tabla -->
        <tbody>
          <tr v-for="(articulo, index) in carrito" :key="index">
            <td>{{ index + 1 }}</td>
            <td>{{ articulo.nombre }}</td>
            <td>{{ articulo.cantidad }}</td>
            <td>{{ articulo.valor }}</td>
            <td>{{ articulo.valorTotal }}</td>
            <td><button @click="eliminarArticulo(index)">Eliminar</button></td>
          </tr>
        </tbody>
      </table>
      <!-- Resumen de la compra -->
      <div>
        <p>Total Compra: {{ totalCompra }}</p>
        <p>Descuento: {{ descuento }}</p>
        <p>Total a Pagar: {{ totalConDescuento }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        nombre: '',
        cantidad: 0,
        valor: 0,
        carrito: [],
      };
    },
    computed: {
      totalCompra() {
        // Calcular el total de la compra
        return this.carrito.reduce((total, articulo) => total + articulo.valorTotal, 0);
      },
      descuento() {
        // Calcular el descuento aplicable
        let descuentoPorCantidad = 0;
        let descuentoPorPrecio = 0;
  
        if (this.totalCompra >= 120000) {
          descuentoPorPrecio = 0.1;
        } else if (this.totalCompra >= 60000) {
          descuentoPorPrecio = 0.05;
        }
  
        if (this.carrito.length >= 12) {
          descuentoPorCantidad = 0.2;
        } else if (this.carrito.length >= 6) {
          descuentoPorCantidad = 0.1;
        }
  
        // Seleccionar el mayor descuento
        const mayorDescuento = Math.max(descuentoPorCantidad, descuentoPorPrecio);
  
        // Aplicar el descuento al total
        return mayorDescuento * this.totalCompra;
      },
      totalConDescuento() {
        // Calcular el total a pagar con el descuento aplicado
        return this.totalCompra - this.descuento;
      },
    },
    methods: {
      agregarArticulo() {
        // Agregar el artículo al carrito
        this.carrito.push({
          nombre: this.nombre,
          cantidad: this.cantidad,
          valor: this.valor,
          valorTotal: this.cantidad * this.valor,
        });
  
        // Limpiar los campos del formulario
        this.nombre = '';
        this.cantidad = 0;
        this.valor = 0;
      },
      eliminarArticulo(index) {
        // Eliminar un artículo del carrito
        this.carrito.splice(index, 1);
      },
    },
  };
  </script>
  
  <style scoped>
  /* Estilos CSS personalizados */
  </style>
  
