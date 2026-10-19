<script setup>

import {Factory, Ruler, Wrench, Crown} from "lucide-vue-next";

defineProps({
    items: {
        type: Array,
        default: () => [
            {titulo: 'Confecção Própria', descricao: 'Produzido por nós, garantindo qualidade e exclusividade', icone: Factory},
            {titulo: 'Sob-Medida', descricao: 'Feito nas medidas exatas do seu ambiente, sem adaptações.', icone: Ruler},
            {titulo: 'Nós Instalamos', descricao: 'Instalação profissional feita pela nossa equipe especializada.', icone: Wrench},
            {titulo: 'Exclusivo', descricao: 'Você escolhe modelo, tecido e detalhes do jeito que quiser.', icone: Crown},
        ],
    },
});
</script>

<template>
    <section
        aria-label="Nossos diferenciais"
        class="border-y border-gray-200 bg-pleats px-6 py-8 md:py-10"
    >
        <ul
            class="mx-auto grid max-w-7xl grid-cols-2 gap-x-6 gap-y-6 md:grid-cols-4"
        >
            <li
                v-for="(item, index) in items"
                :key="item"
                class="flex flex-col items-center justify-center gap-3 text-center lg:border-r lg:border-gray-200 lg:last:border-r-0"
            >
                <component :is="item.icone" :size="48" class="text-primary-600" />
                <p class="text-primary-600 font-medium">{{ item.titulo }}</p>
                <span class="text-neutral-600 font-light text-sm max-w-64">{{ item.descricao }}</span>
            </li>
        </ul>
    </section>
</template>
