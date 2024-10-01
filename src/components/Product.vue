<script setup>
import { computed } from 'vue';


defineEmits(["add-to-cart", "increase-quantity", "decrease-quantity"])

const props = defineProps({
    product: {
        type: Object, required: true
    },
    cart: {
        type: Array, required: true
    }
})

const existsProduct = computed(() => {
    return props.cart.findIndex(e => e.id === props.product.id) >= 0
})

</script>

<template>
    <div>
        <div class="relative flex justify-center">
            <img class="rounded-lg" :src="`/images/${product.image}.jpg`" :alt="`Imagen postre ${product.name}`">

            <div 
                v-if="existsProduct"
                class="flex items-center justify-center gap-2 text-white font-bold bg-[#BC3C12] hover:scale-110 transition-transform rounded-full px-6 py-3   absolute bottom-[-20px]">

                <button 
                    class="border border-white w-5 h-5 flex justify-center items-center rounded-full"
                    @click="$emit('increase-quantity', product.id)"
                >+</button>
                <span>{{ product.quantity }}</span>
                <button 
                    class="border border-white w-5 h-5 flex justify-center items-center rounded-full"
                    @click="$emit('decrease-quantity', product.id)"
                >-</button>
                
        </div>

            <button v-else @click="$emit('add-to-cart', product)"
                class="flex items-center justify-center gap-2 text-[#6e615f] font-bold bg-white hover:scale-110 transition-transform rounded-full px-6 py-3 border border-[#6e615f] absolute bottom-[-20px]">
                <img src="/images/icon-add-to-cart.svg" alt="Icono cart">
                Añadir al carrito
            </button>

        </div>
        <div class="mt-10 space-y-2">
            <span class="text-[#6e615f]">{{ product.category }}</span>
            <p class="text-[#6e615f] font-extrabold">{{ product.name }}</p>
            <p class="text-[#BC3C12] font-bold">${{ product.price }}</p>
        </div>
    </div>
</template>