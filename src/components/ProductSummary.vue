<script setup>

defineEmits(["delete-product"])

defineProps({
    product: {
        type: Object,
        required: true
    }
})

</script>

<template>
    <div class="border-b border-b-[#94868452] pb-3 mb-4 overflow-auto">
        <div class="flex items-center justify-between">
            <div>
                <p class="text-[#6e615f] font-extrabold">{{ product.name }}</p>
                <div class="flex items-center gap-4 mt-2">
                    <span class="text-[#BC3C12] font-bold">{{ product.quantity }}x</span>
                    <p class="text-[#6e615f] font-normal">${{ product.price }}</p>
                </div>
            </div>
            <div @click="$emit('delete-product', product.id)" class="border border-[#6e615f] rounded-full max-w-max p-1 cursor-pointer">
                <img src="/images/icon-remove-item.svg" alt="Icono eliminar">
            </div>
        </div>
        <div></div>
    </div>
</template>