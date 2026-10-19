
<script setup>
import { ref } from 'vue';
import { ChevronDown } from 'lucide-vue-next';

defineProps({
    title: {
        type: String,
        default: 'Mais do que cortinas, criamos ambientes.',
    },
    description: {
        type: String,
        default:
            'Acreditamos que cada detalhe faz diferença na forma como vivemos nossos espaços. Por isso, unimos cuidado, qualidade e atenção às necessidades de cada cliente.',
    },
    image: {
        type: String,
        required: true,
    },
    imageAlt: {
        type: String,
        default: 'Ambiente decorado com cortinas sob medida',
    },
    topics: {
        type: Array,
        default: () => [
            {
                title: 'Nossa história',
                description:
                    'Conte aqui como a empresa surgiu e o que inspira o trabalho da equipe.',
            },
            {
                title: 'Nosso compromisso',
                description:
                    'Explique como a empresa trabalha para entregar qualidade e atenção em cada projeto.',
            },
            {
                title: 'Atendimento personalizado',
                description:
                    'Apresente como vocês ajudam cada cliente a encontrar a solução ideal para seu ambiente.',
            },
        ],
    },
});

const openTopic = ref(0);

function toggleTopic(index) {
    openTopic.value = openTopic.value === index ? null : index;
}
</script>

<template>
    <section id="sobre" class="px-6 py-20 md:py-28 bg-light">
        <div
            class="mx-auto grid max-w-7xl items-center gap-12 lg:grid-cols-2 lg:gap-20"
        >
            <!-- Imagem -->
            <div class="relative">
                <img
                    :src="image"
                    :alt="imageAlt"
                    class="aspect-4/5 w-full rounded-2xl object-cover"
                    loading="lazy"
                />

                <div
                    class="pointer-events-none absolute inset-0 rounded-2xl ring-1 ring-inset ring-black/5"
                />
            </div>

            <!-- Conteúdo -->
            <div>
                <span
                    class="text-sm font-semibold uppercase tracking-[0.2em] text-neutral-500"
                >
                    Sobre nós
                </span>

                <h2
                    class="mt-4 text-3xl font-bold tracking-tight md:text-5xl"
                >
                    {{ title }}
                </h2>

                <p class="mt-6 leading-8 text-neutral-600">
                    {{ description }}
                </p>

                <!-- Informações expansíveis -->
                <div class="mt-9 divide-y divide-neutral-200 border-y border-neutral-200">
                    <div
                        v-for="(topic, index) in topics"
                        :key="topic.title"
                        class="py-5"
                    >
                        <h3>
                            <button
                                :id="`about-trigger-${index}`"
                                type="button"
                                class="flex w-full items-center justify-between gap-4 text-left font-semibold cursor-pointer text-neutral-900"
                                :aria-expanded="openTopic === index"
                                :aria-controls="`about-panel-${index}`"
                                @click="toggleTopic(index)"
                            >
                                {{ topic.title }}

                                <ChevronDown
                                    :size="20"
                                    class="shrink-0 transition-transform duration-200"
                                    :class="openTopic === index ? 'rotate-180' : ''"
                                />
                            </button>
                        </h3>

                        <div
                            :id="`about-panel-${index}`"
                            role="region"
                            :aria-labelledby="`about-trigger-${index}`"
                            class="grid transition-all duration-300 ease-out"
                            :style="{
                                gridTemplateRows: openTopic === index ? '1fr' : '0fr',
                                opacity: openTopic === index ? '1' : '0'
                            }"
                        >
                            <div class="overflow-hidden leading-7 text-neutral-600 pt-3">
                                {{ topic.description }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>
