<script setup>
import SiteFooter from './SiteFooter.vue';
import BaseButton from "../Components/BaseButton.vue";
import {MoveUpRight} from "lucide-vue-next";
import { ref } from 'vue'
import { Link, usePage } from '@inertiajs/vue3'

const aberto = ref(false)
const page = usePage()

const links = [
    { label: 'Início', to: '/' },
    { label: 'Cortinas', to: '/cortinas' },
    { label: 'Persianas', to: '/persianas' },
    { label: 'Contato', to: '/#contato', ancora: true },
]

const estaAtivo = (link) => {
    if (link.ancora) return false
    if (link.to === '/') return page.url === '/'
    return page.url.startsWith(link.to)
}

</script>


<template>
    <div class="min-h-screen bg-white text-gray-900">
        <header class="sticky top-0 z-50 glass bg-white/80">
            <div class="mx-auto flex max-w-7xl items-center justify-between px-6 py-4">
                <Link
                    href="/"
                    class="flex items-center gap-1.5 text-xl font-semibold tracking-tight text-gray-900 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary-600 focus-visible:ring-offset-2"
                >
                    <span class="text-sm text-primary-600" aria-hidden="true">✦</span>
                    Costurart
                </Link>

                <nav class="hidden items-center gap-8 md:flex" aria-label="Principal">
                    <component
                        :is="link.ancora ? 'a' : Link"
                        v-for="link in links"
                        :key="link.to"
                        :href="link.to"
                        :aria-current="estaAtivo(link) ? 'page' : undefined"
                        class="group relative py-1 font-medium transition-colors hover:text-primary-600 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-primary-600 focus-visible:ring-offset-2"
                        :class="estaAtivo(link) ? 'text-primary-600' : 'text-gray-700'"
                    >
                        {{ link.label }}
                        <span
                            class="absolute -bottom-0.5 left-0 h-0.5 w-full origin-left rounded-full bg-primary-600 transition-transform duration-200 group-hover:scale-x-100"
                            :class="estaAtivo(link) ? 'scale-x-100' : 'scale-x-0'"
                            aria-hidden="true"
                        />
                    </component>

                    <BaseButton href="https://wa.me/55SEUNUMERO" variant="solid" size="sm" rounded>
                        Solicite um orçamento
                    </BaseButton>
                </nav>

                <button
                    type="button"
                    class="flex h-10 w-10 items-center justify-center rounded-lg text-gray-700 transition-colors hover:bg-primary-600/10 hover:text-primary-600 md:hidden"
                    :aria-expanded="aberto"
                    aria-controls="menu-mobile"
                    aria-label="Abrir menu"
                    @click="aberto = !aberto"
                >
                    <svg class="h-6 w-6" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" aria-hidden="true">
                        <path v-if="!aberto" d="M4 7h16M4 12h16M4 17h16" />
                        <path v-else d="M6 6l12 12M18 6L6 18"/>
                    </svg>
                </button>
            </div>

            <nav
                v-show="aberto"
                id="menu-mobile"
                class="border-t border-primary-600/10 bg-white px-6 py-4 md:hidden"
                aria-label="Principal (mobile)"
            >
                <div class="flex flex-col gap-1">
                    <component
                        :is="link.ancora ? 'a' : Link"
                        v-for="link in links"
                        :key="link.to"
                        :href="link.to"
                        :aria-current="estaAtivo(link) ? 'page' : undefined"
                        class="rounded-lg px-3 py-3 text-base font-medium transition-colors hover:bg-primary-600/10 hover:text-primary-600"
                        :class="estaAtivo(link) ? 'bg-primary-600/10 text-primary-600' : 'text-gray-700'"
                        @click="aberto = false"
                    >
                        {{ link.label }}
                    </component>
                </div>

                <BaseButton href="https://wa.me/55SEUNUMERO" variant="solid" rounded class="w-full">
                    Solicite um orçamento
                </BaseButton>
            </nav>
        </header>

        <main>
            <slot />
        </main>

        <SiteFooter />
    </div>
</template>
