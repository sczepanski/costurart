<script setup>
import { computed, nextTick, onBeforeUnmount, onMounted, ref, watch } from 'vue';
import { ChevronLeft, ChevronRight, MoveUpRight } from 'lucide-vue-next';
import BaseButton from "./BaseButton.vue";

const props = defineProps({
    id: {
        type: String,
        default: 'modelos'
    },
    title: {
        type: String,
        default: 'Encontre seu estilo',
    },
    description: {
        type: String,
        default: 'Explore nossa coleção e descubra os detalhes que transformam cada ambiente.',
    },
    models: {
        type: Array,
        default: () => [],
    },
    autoplay: {
        type: Number,
        default: 0,
    },
    showFilter: {
        type: Boolean,
        default: true,
    },
});

const categories = ['Todos', 'Cortinas', 'Persianas', 'Acessórios'];
const activeCategory = ref('Todos');
const carousel = ref(null);
const activePage = ref(0);
const pageCount = ref(1);
const itemsPerPage = ref(1);

const filteredModels = computed(() => {
    if (activeCategory.value === 'Todos') {
        return props.models;
    }

    return props.models.filter(
        (model) => model.category === activeCategory.value,
    );
});

// ---------- Navegação ----------

function updatePosition() {
    const el = carousel.value;
    if (!el) return;

    const totalItems = filteredModels.value.length;
    const carouselWidth = el.clientWidth;

    if (carouselWidth >= 1024) itemsPerPage.value = 3;
    else if (carouselWidth >= 640) itemsPerPage.value = 2;
    else itemsPerPage.value = 1;

    pageCount.value = Math.max(1, Math.ceil(totalItems / itemsPerPage.value));

    const itemWidth = el.children[0]?.offsetWidth || carouselWidth;
    const gap = 20;
    const pageWidth = (itemWidth + gap) * itemsPerPage.value;
    const maxScroll = el.scrollWidth - el.clientWidth;

    // No fim do scroll, a última página pode não alinhar à esquerda
    if (el.scrollLeft >= maxScroll - 2) {
        activePage.value = pageCount.value - 1;
        return;
    }

    activePage.value = Math.min(
        Math.round(el.scrollLeft / pageWidth),
        pageCount.value - 1,
    );
}

function goToPage(index) {
    const el = carousel.value;
    if (!el) return;

    const itemWidth = el.children[0]?.offsetWidth || el.clientWidth;
    const gap = 20;
    const pageWidth = (itemWidth + gap) * itemsPerPage.value;
    const maxScroll = el.scrollWidth - el.clientWidth;

    el.scrollTo({
        left: Math.min(index * pageWidth, maxScroll),
        behavior: 'smooth',
    });
}

function go(index) {
    // Carrossel infinito
    if (index < 0) {
        index = pageCount.value - 1;
    } else if (index >= pageCount.value) {
        index = 0;
    }

    goToPage(index);
    startAutoplay();
}

// ---------- Autoplay ----------

let timer = null;
let hovering = false;
let focused = false;
let touching = false;

function stopAutoplay() {
    clearInterval(timer);
    timer = null;
}

function startAutoplay() {
    stopAutoplay();

    if (
        !props.autoplay ||
        pageCount.value < 2 ||
        hovering ||
        focused ||
        touching
    ) {
        return;
    }

    if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
        return;
    }

    timer = setInterval(() => {
        const nextPage =
            activePage.value >= pageCount.value - 1
                ? 0
                : activePage.value + 1;

        goToPage(nextPage);
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

function onFocusIn() {
    focused = true;
    stopAutoplay();
}

function onFocusOut(event) {
    if (!event.currentTarget.contains(event.relatedTarget)) {
        focused = false;
        startAutoplay();
    }
}

function onTouchStart() {
    touching = true;
    stopAutoplay();
}

function onTouchEnd() {
    touching = false;
    startAutoplay();
}

// ---------- Transição entre categorias ----------

// Ao trocar de categoria, só pausamos o autoplay.
// O novo carrossel é criado pelo <Transition> (por causa do :key)
// e já nasce na posição inicial, então não precisa zerar o scroll.
watch(activeCategory, () => {
    stopAutoplay();
    activePage.value = 0;
});

// Chamado quando a animação de entrada termina:
// só agora o novo carrossel existe e está com o tamanho final.
function onAfterEnter() {
    updatePosition();
    startAutoplay();
}

// ---------- Ciclo de vida ----------

watch(
    () => [props.models.length, props.autoplay],
    async () => {
        await nextTick();
        updatePosition();
        startAutoplay();
    },
);

onMounted(async () => {
    await nextTick();
    updatePosition();
    startAutoplay();

    window.addEventListener('resize', updatePosition);
});

onBeforeUnmount(() => {
    stopAutoplay();
    window.removeEventListener('resize', updatePosition);
});
</script>

<template>
    <section :id="id" class="bg-dark-vignette overflow-hidden px-6 py-16 md:py-20">
        <div class="mx-auto max-w-7xl">
            <!-- Cabeçalho -->
            <header class="mx-auto max-w-2xl text-center">
                <span
                    class="text-sm font-semibold uppercase tracking-[0.2em] text-neutral-300"
                >
                    Nossa coleção
                </span>

                <h2 class="mt-4 text-3xl font-bold tracking-tight md:text-5xl text-neutral-100">
                    {{ title }}
                </h2>

                <p class="mt-5 leading-8 text-neutral-400">
                    {{ description }}
                </p>
            </header>

            <!-- Filtros -->
            <nav
                v-if="showFilter"
                class="mt-9 flex flex-wrap justify-center gap-3"
                aria-label="Filtrar modelos"
            >
                <BaseButton
                    v-for="category in categories"
                    :key="category"
                    type="button"
                    rounded
                    :variant="activeCategory === category ? 'solid' : 'outline'"
                    :aria-pressed="activeCategory === category"
                    class="px-5 py-2 text-sm font-medium text-white"
                    @click="activeCategory = category"
                >
                    {{ category }}
                </BaseButton>
            </nav>

            <!-- Carrossel -->
            <div
                class="group/carousel relative mt-10"
                @mouseenter="onMouseEnter"
                @mouseleave="onMouseLeave"
                @focusin="onFocusIn"
                @focusout="onFocusOut"
                @touchstart.passive="onTouchStart"
                @touchend.passive="onTouchEnd"
                @touchcancel.passive="onTouchEnd"
            >
                <!--
                    mode="out-in": o carrossel antigo sai antes do novo entrar.
                    :key="activeCategory": faz o Vue tratar cada categoria como um elemento novo,
                    que é o que dispara a animação.
                    :duration: tempo total de entrada (inclui a cascata dos cards).
                -->
                <Transition
                    name="gallery"
                    mode="out-in"
                    :duration="{ enter: 900, leave: 250 }"
                    @after-enter="onAfterEnter"
                >
                    <div
                        v-if="filteredModels.length"
                        :key="activeCategory"
                        ref="carousel"
                        class="flex snap-x snap-mandatory gap-5 overflow-x-auto scroll-smooth [scrollbar-width:none] [&::-webkit-scrollbar]:hidden"
                        aria-label="Galeria de modelos"
                        @scroll.passive="updatePosition"
                    >
                        <!-- --i é usado no CSS para atrasar cada card (efeito cascata) -->
                        <article
                            v-for="(model, index) in filteredModels"
                            :key="model.id"
                            :style="{ '--i': index }"
                            class="w-[85%] shrink-0 snap-start sm:w-[calc(50%-0.625rem)] lg:w-[calc(33.333%-0.834rem)]"
                        >
                            <div
                                class="group/card relative overflow-hidden rounded-2xl bg-neutral-100"
                            >
                                <img
                                    :src="model.image"
                                    :alt="model.alt || model.name"
                                    class="aspect-[4/5] w-full object-cover transition-transform duration-500 group-hover/card:scale-105"
                                    loading="lazy"
                                    draggable="false"
                                />

                                <!-- Legenda sobre a imagem -->
                                <div
                                    class="absolute inset-x-0 bottom-0 bg-gradient-to-t from-black/70 via-black/20 to-transparent p-5 pt-16 text-white"
                                >
                                    <p class="text-xs uppercase tracking-widest text-white/75">
                                        {{ model.category }}
                                    </p>

                                    <h3 class="mt-1 text-lg font-semibold">
                                        {{ model.name }}
                                    </h3>
                                </div>

                                <!-- Ação opcional -->
                                <a
                                    v-if="model.href"
                                    :href="model.href"
                                    :aria-label="`Conheça ${model.name}`"
                                    class="absolute right-4 top-4 flex size-10 items-center justify-center rounded-full border border-white/70 bg-black/10 text-white backdrop-blur-sm transition hover:bg-white hover:text-neutral-900"
                                >
                                    <MoveUpRight :size="20" />
                                </a>
                            </div>
                        </article>
                    </div>

                    <!-- Estado vazio: precisa de uma key diferente para o Vue animar a troca -->
                    <p
                        v-else
                        key="empty"
                        class="min-h-80 rounded-2xl bg-neutral-100 px-6 py-16 text-center text-neutral-500"
                    >
                        Nenhum modelo cadastrado nesta categoria ainda.
                    </p>
                </Transition>

                <!-- Setas -->
                <template v-if="pageCount > 1">
                    <button
                        type="button"
                        aria-label="Modelos anteriores"
                        class="absolute left-3 top-1/2 hidden size-11 -translate-y-1/2 items-center justify-center rounded-full button-outline text-white/80 transition cursor-pointer md:flex md:opacity-0 md:group-hover/carousel:opacity-100"
                        @click="go(activePage - 1)"
                    >
                        <ChevronLeft :size="24" />
                    </button>

                    <button
                        type="button"
                        aria-label="Próximos modelos"
                        class="absolute right-3 top-1/2 hidden size-11 -translate-y-1/2 items-center justify-center rounded-full button-outline text-white/80 transition cursor-pointer md:flex md:opacity-0 md:group-hover/carousel:opacity-100"
                        @click="go(activePage + 1)"
                    >
                        <ChevronRight :size="24" />
                    </button>
                </template>
            </div>

            <!-- Indicadores -->
            <div
                v-if="pageCount > 1"
                class="mt-6 flex items-center justify-center gap-2"
                aria-label="Páginas do carrossel"
            >
                <button
                    v-for="page in pageCount"
                    :key="page"
                    type="button"
                    :aria-label="`Ir para página ${page}`"
                    :aria-current="activePage === page - 1 ? 'true' : undefined"
                    class="h-2 rounded-full transition-all duration-300"
                    :class="
                        activePage === page - 1
                            ? 'w-6 bg-neutral-600'
                            : 'w-2 bg-neutral-300 hover:bg-neutral-400'
                    "
                    @click="go(page - 1)"
                />
            </div>
        </div>
    </section>
</template>

<style scoped>
/* ----- SAÍDA: o carrossel antigo desaparece rápido ----- */
.gallery-leave-active {
    transition: opacity 250ms ease, transform 250ms ease;
}
.gallery-leave-to {
    opacity: 0;
    transform: translateY(-8px);
}

/* ----- ENTRADA: cada card sobe e aparece, um depois do outro ----- */
.gallery-enter-active article {
    transition:
        opacity 500ms ease,
        transform 500ms cubic-bezier(0.22, 1, 0.36, 1);
    /* --i vem do :style de cada card (0, 1, 2, ...) */
    transition-delay: calc(var(--i) * 70ms);
}
.gallery-enter-from article {
    opacity: 0;
    transform: translateY(20px) scale(0.97);
}

/* ----- ESTADO VAZIO: fade simples (é um <p>, não tem article dentro) ----- */
p.gallery-enter-active {
    transition: opacity 300ms ease;
}
p.gallery-enter-from {
    opacity: 0;
}

/* Quem desativou animações no sistema vê a troca direta */
@media (prefers-reduced-motion: reduce) {
    .gallery-leave-active,
    .gallery-enter-active article,
    p.gallery-enter-active {
        transition: none;
        transition-delay: 0ms;
    }
}
</style>
