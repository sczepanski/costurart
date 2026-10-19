<script setup>
import {onBeforeUnmount, onMounted, ref, watch} from 'vue';
import {ChevronLeft, ChevronRight, MoveUpRight, Plus} from "lucide-vue-next";
import BaseButton from './BaseButton.vue';

const props = defineProps({
    id: {
        type: String,
        default: 'cortina',
    },
    eyebrow: {
        type: String,
        default: '',
    },
    title: {
        type: String,
        default: 'Cortinas sob medida',
    },
    description: {
        type: String,
        default:
            'Transforme seus ambientes com cortinas feitas para combinar com seu estilo, suas necessidades e seu espaço.',
    },
    models: {
        type: Array,
        default: () => [
            {nome: 'Wave', icone: '/assets/icons/wave.svg'},
            {nome: 'Franzida', icone: '/assets/icons/franzido.svg'},
            {nome: 'Romana', icone: '/assets/icons/romana.svg'},
            {nome: 'Painel', icone: '/assets/icons/painel.svg'},
            {nome: 'Prega', icone: '/assets/icons/prega.svg'},
            {nome: 'Bando', icone: '/assets/icons/bando.svg'},
            {nome: 'Xale', icone: '/assets/icons/xale.svg'},
            {nome: 'Rolo', icone: '/assets/icons/rolo.svg'},
        ],
    },
    images: {
        type: Array,
        default: () => [],
    },
    // Segundos entre cada avanço. 0 = autoplay desligado
    autoplay: {
        type: Number,
        default: 0,
    },
});

const carousel = ref(null);
const active = ref(0);

/* ---------- Posição / navegação ---------- */

function onScroll() {
    const el = carousel.value;
    if (!el) return;

    const slides = Array.from(el.children);
    if (!slides.length) return;

    // Fim do scroll: o último slide pode nunca alinhar à esquerda (largura de 85%)
    if (el.scrollLeft + el.clientWidth >= el.scrollWidth - 2) {
        active.value = slides.length - 1;
        return;
    }

    const left = el.getBoundingClientRect().left;
    let closest = 0;
    let min = Infinity;

    slides.forEach((slide, i) => {
        const distance = Math.abs(slide.getBoundingClientRect().left - left);
        if (distance < min) {
            min = distance;
            closest = i;
        }
    });

    active.value = closest;
}

function goTo(index) {
    const el = carousel.value;
    if (!el) return;

    const slides = Array.from(el.children);
    const target = slides[Math.max(0, Math.min(index, slides.length - 1))];
    if (!target) return;

    const offset = target.getBoundingClientRect().left - el.getBoundingClientRect().left;
    el.scrollTo({left: el.scrollLeft + offset, behavior: 'smooth'});
}

// Navegação manual (setas e pontinhos): move e reinicia a contagem do autoplay
function go(index) {
    goTo(index);
    startAutoplay();
}

/* ---------- Autoplay ---------- */

let timer = null;
let hovering = false;

function stopAutoplay() {
    clearInterval(timer);
    timer = null;
}

function startAutoplay() {
    stopAutoplay();

    if (!props.autoplay || props.images.length < 2 || hovering) return;
    if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return;

    timer = setInterval(() => {
        const last = props.images.length - 1;
        goTo(active.value >= last ? 0 : active.value + 1); // chegou no fim: volta para o início
    }, props.autoplay * 1000);
}

function onMouseEnter() {
    hovering = true;
    stopAutoplay();
}

function onMouseLeave() {
    hovering = false;
    startAutoplay();
}

/* ---------- Ciclo de vida ---------- */

onMounted(() => {
    onScroll();
    startAutoplay();
});

onBeforeUnmount(() => {
    stopAutoplay();
});

watch(() => [props.autoplay, props.images.length], startAutoplay);
</script>

<template>
    <section
        :id="id"
        class="px-6 py-20 md:py-28 bg-light"
    >
        <div
            class="mx-auto grid max-w-7xl items-center gap-y-8 lg:grid-cols-3 lg:gap-x-16"
        >
            <!-- Coluna de conteúdo -->
            <div class="min-w-0 lg:col-span-2">
                <div
                    class="text-sm font-semibold uppercase tracking-widest text-neutral-500 flex gap-1 items-center"
                >
                    <span class="text-sm text-primary-600" aria-hidden="true">✦</span>
                    {{ eyebrow }}
                </div>

                <h2
                    class="mt-4 text-3xl font-bold tracking-tight md:text-5xl"
                >
                    {{ title }}
                </h2>

                <p class="mt-2 max-w-xl text-base text-neutral-600">
                    {{ description }}
                </p>

                <!-- Modelos disponíveis -->
                <p class="mt-8 text-sm font-semibold uppercase tracking-widest text-neutral-500">
                    Disponíveis nos seguintes modelos
                </p>
                <section class="mt-4 flex flex-wrap items-center gap-3">
                    <div
                        v-for="model in models"
                        :key="model.nome"
                        :to="model.rota"
                        class="group relative flex h-24 w-24 flex-col items-center justify-center gap-1.5 rounded-xl border border-primary-600/20 bg-white p-2 shadow-md shadow-primary-600/10 transition-all duration-200 ease-out hover:-translate-y-1 hover:border-primary-600 hover:shadow-lg hover:shadow-primary-600/25 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary-600 focus-visible:ring-offset-2 cursor-pointer"
                    >
                        <img
                            :src="model.icone"
                            :alt="model.nome"
                            class="h-12 w-12 transition-transform duration-200 group-hover:scale-110"
                        />
                        <span class="text-sm font-medium text-primary-600">{{ model.nome }}</span>

                        <!-- seta que aparece no hover -->
                        <span
                            class="absolute right-1.5 top-1.5 text-primary-600 opacity-0 transition-opacity duration-200 group-hover:opacity-100"
                            aria-hidden="true"
                        >
            ↗
                        </span>
                    </div>
                </section>

                <div class="mt-8">
                    <BaseButton href="#modelos" variant="solid">
                        Conheça os modelos
                        <MoveUpRight :size="20" :stroke-width="2"/>
                    </BaseButton>
                </div>
            </div>

            <!-- Coluna do carrossel -->
            <div class="min-w-0 lg:col-span-1">
                <div
                    v-if="images.length"
                    class="group relative"
                    @mouseenter="onMouseEnter"
                    @mouseleave="onMouseLeave"
                    @focusin="stopAutoplay"
                    @focusout="startAutoplay"
                >
                    <div
                        ref="carousel"
                        class="flex snap-x snap-mandatory gap-4 overflow-x-auto scroll-smooth
                               [scrollbar-width:none] [&::-webkit-scrollbar]:hidden"
                        aria-label="Galeria de cortinas"
                        @scroll.passive="onScroll"
                        @touchstart.passive="stopAutoplay"
                        @touchend.passive="startAutoplay"
                    >
                        <figure
                            v-for="image in images"
                            :key="image.src"
                            class="relative w-full shrink-0 snap-start overflow-hidden rounded-2xl"
                        >
                            <img
                                :src="image.src"
                                :alt="image.alt"
                                draggable="false"
                                class="aspect-4/5 w-full object-cover"
                                loading="lazy"
                            />

                            <figcaption
                                v-if="image.caption"
                                class="dark-glass absolute inset-x-0 bottom-0 p-6 text-center font-medium text-white"
                            >
                                {{ image.caption }}
                            </figcaption>

                            <a class="absolute flex items-center gap-2 right-2 top-2 button-outline cursor-pointer rounded-full text-white px-3 py-1.5">
                                <span class="text-sm md:text-xs">
                                    Ver detalhes
                                </span>
                                <Plus
                                    :size="20"
                                    :stroke-width="2"
                                />
                            </a>

                        </figure>
                    </div>

                    <!-- Setas (só desktop, aparecem no hover) -->
                    <template v-if="images.length > 1">
                        <button
                            type="button"
                            aria-label="Imagem anterior"
                            :disabled="active === 0"
                            class="absolute left-3 top-1/2 hidden size-10 -translate-y-1/2 items-center justify-center rounded-full md:flex transition"
                            :class="active === 0
                                ? 'pointer-events-none opacity-0'
                                : 'opacity-0 md:group-hover:opacity-100'"
                            @click="goTo(active - 1)"
                        >
                            <ChevronLeft :size="32" :stroke-width="2" class="button-outline cursor-pointer rounded-full p-1 text-white/80"/>
                        </button>

                        <button
                            type="button"
                            aria-label="Próxima imagem"
                            :disabled="active === images.length - 1"
                            class="absolute right-3 top-1/2 hidden size-10 -translate-y-1/2 items-center justify-center md:flex transition"
                            :class="active === images.length - 1
                                ? 'pointer-events-none opacity-0'
                                : 'opacity-0 md:group-hover:opacity-100'"
                            @click="goTo(active + 1)"
                        >
                            <ChevronRight :size="32" :stroke-width="2" class="button-outline cursor-pointer rounded-full p-1 text-white/80"/>
                        </button>
                    </template>

                    <!-- Indicadores -->
                    <div
                        v-if="images.length > 1"
                        class="mt-4 flex items-center justify-center gap-2"
                    >
                        <button
                            v-for="(image, i) in images"
                            :key="image.src"
                            type="button"
                            :aria-label="`Ir para imagem ${i + 1}`"
                            :aria-current="active === i"
                            class="h-2 rounded-full transition-all duration-300"
                            :class="active === i
                                ? 'w-6 bg-neutral-800'
                                : 'w-2 bg-neutral-300 hover:bg-neutral-400'"
                            @click="goTo(i)"
                        />
                    </div>
                </div>

                <!-- Estado temporário enquanto não houver imagens -->
                <div
                    v-else
                    class="flex aspect-4/5 items-center justify-center rounded-2xl bg-neutral-100 p-8 text-center text-neutral-500"
                >
                    Adicione imagens de cortinas para visualizar o carrossel.
                </div>
            </div>
        </div>
    </section>
</template>
