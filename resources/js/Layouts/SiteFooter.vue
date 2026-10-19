```vue
<script setup>
const anoAtual = new Date().getFullYear();

const links = [
    { label: 'Início', href: '#inicio' },
    { label: 'Cortinas', href: '#cortinas' },
    { label: 'Persianas', href: '#persianas' },
    { label: 'Modelos', href: '#modelos' },
    { label: 'Sobre nós', href: '#sobre' },
];
</script>

<template>
    <footer class="bg-dark-vignette text-white">
        <div class="mx-auto grid max-w-7xl gap-10 px-6 py-12 sm:grid-cols-2 lg:grid-cols-4">

            <!-- Identidade da loja -->
            <div>
                <a href="#inicio" class="text-xl font-bold tracking-wide">
                    Costurart
                </a>

                <p class="mt-4 max-w-xs text-sm leading-6 text-gray-400">
                    Cortinas, persianas e acessórios para transformar
                    ambientes com conforto, elegância e personalidade.
                </p>
            </div>

            <!-- Navegação -->
            <div>
                <h2 class="text-sm font-semibold uppercase tracking-wider">
                    Navegação
                </h2>

                <ul class="mt-4 space-y-3">
                    <li v-for="link in links" :key="link.href">
                        <a
                            :href="link.href"
                            class="text-sm text-gray-400 transition-colors duration-200 hover:text-white"
                        >
                            {{ link.label }}
                        </a>
                    </li>
                </ul>
            </div>

            <!-- Contato -->
            <div>
                <h2 class="text-sm font-semibold uppercase tracking-wider">
                    Entre em contato
                </h2>

                <div class="mt-4 space-y-3 text-sm text-gray-400">
                    <p>Telefone: (XX) XXXXX-XXXX</p>
                    <p>E-mail: contato@exemplo.com</p>
                    <p>Endereço da loja</p>
                </div>
            </div>

            <!-- Atendimento -->
            <div>
                <h2 class="text-sm font-semibold uppercase tracking-wider">
                    Atendimento
                </h2>

                <p class="mt-4 text-sm leading-6 text-gray-400">
                    Entre em contato com nossa equipe e encontre
                    as melhores soluções para o seu ambiente.
                </p>

                <a
                    href="#contato"
                    class="mt-5 inline-flex items-center rounded-md border border-white/30 px-4 py-2 text-sm font-medium transition-colors duration-200 hover:bg-white hover:text-gray-950"
                >
                    Solicitar orçamento
                </a>
            </div>
        </div>

        <!-- Faixa inferior -->
        <div class="border-t border-white/10">
            <div class="mx-auto flex max-w-7xl flex-col gap-2 px-6 py-5 text-xs text-gray-500 sm:flex-row sm:items-center sm:justify-between">
                <p>
                    © {{ anoAtual }} Costurart. Todos os direitos reservados.
                </p>

                <p>Feito com cuidado para o seu ambiente.</p>
            </div>
        </div>
    </footer>
</template>
```
