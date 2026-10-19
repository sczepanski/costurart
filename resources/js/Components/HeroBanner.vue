<script setup>
import BaseButton from "./BaseButton.vue";

defineProps({
    title: {
        type: String,
        default: 'Transforme seu ambiente.',
    },
    subtitle: {
        type: String,
        default: 'Cortinas e persianas sob medida para você.',
    },
    image: {
        type: String,
        required: true,
    },
    buttonText: {
        type: String,
        default: 'Conheça nossos modelos',
    },
    buttonLink: {
        type: String,
        default: '#produtos',
    },
});
</script>

<template>
    <section
        class="relative flex min-h-200 items-center bg-cover bg-center px-6 py-20"
        :style="{ backgroundImage: `url('${image}')` }"
    >
        <div class="absolute inset-0 bg-black/50"></div>

        <div class="relative mx-auto w-full max-w-7xl">
            <div class="max-w-2xl text-white">
                <h1 class="whitespace-pre-line text-4xl font-bold tracking-tight md:text-6xl">
                    {{ title }}
                </h1>

                <p class="mt-12 max-w-xl text-base leading-relaxed md:text-lg italic tracking-wide">
                    {{ subtitle }}
                </p>

                <BaseButton href="#produtos" variant="solid" class="mt-2" rounded >
                    {{ buttonText }}
                </BaseButton>
            </div>
        </div>
    </section>
</template>
