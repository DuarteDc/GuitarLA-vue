<script setup>

    import { computed } from 'vue';

    const props = defineProps({
        cart: {
            type: Array,
            required: true,
        },
        headerGuitar: {
            type: Object,
            required: true,
        }
    });

    defineEmits(['increase-quantity', 'decrease-quantity', 'add-cart', 'delete-product', 'clean-cart']);

    const totalToPay = computed(()=> props.cart.reduce(( total, product )=> total + ( product.cantidad * product.precio ), 0));   

</script>

<template>
    <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="/">
                        <img class="img-fluid" src="/img/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div class="carrito">
                        <img class="img-fluid" src="/img/carrito.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white p-3">
                            <p 
                                class="text-center m-0"
                                v-if="cart.length === 0"
                            >
                                El carrito esta vacio
                            </p>
                            <div v-else>
                                <table class="w-100 table">
                                    <thead>
                                        <tr>
                                            <th>Imagen</th>
                                            <th>Nombre</th>
                                            <th>Precio</th>
                                            <th>Cantidad</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="product in cart">
                                            <td  class="flex align-items-center">
                                                <img 
                                                    class="img-fluid" 
                                                    :src="'/img/' + product.imagen + '.jpg'" 
                                                    :alt="'imagen guitarra ' + product.nombre"
                                                >
                                            </td>
                                            <td  class="flex align-items-center">{{ product.nombre }}</td>
                                            <td class="fw-bold">
                                                ${{ product.precio }}
                                            </td>
                                            <td class="flex align-items-start gap-4">
                                                <button 
                                                    type="button" 
                                                    class="btn btn-dark"
                                                    @click="$emit('decrease-quantity', product.id)"
                                                >
                                                    -
                                                </button>
                                                {{ product.cantidad }}
                                                <button 
                                                    type="button"
                                                    class="btn btn-dark"
                                                    @click="$emit('increase-quantity', product.id)"
                                                    >
                                                    +
                                                </button>
                                            </td>
                                            <td  class="flex align-items-center">
                                                <button 
                                                    class="btn btn-danger" 
                                                    type="button"
                                                    @click="$emit('delete-product', product.id)"
                                                >
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>

                                <p class="text-end">Total pagar: <span class="fw-bold">${{ totalToPay }}</span></p>
                                <button 
                                    class="btn btn-dark w-100 mt-3 p-2"
                                    @click="$emit('clean-cart')"
                                >
                                    Vaciar Carrito
                                </button>
                            </div>
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo {{ headerGuitar.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">{{ headerGuitar.descripcion }}</p>
                    <p class="text-primary fs-1 fw-black">${{ headerGuitar.precio }}</p>
                    <button 
                        type="button" 
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                        @click="$emit('add-cart', headerGuitar)"
                    >
                        Agregar al Carrito
                    </button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/img/header_guitarra.png" alt="imagen header">
    </header>
</template>
