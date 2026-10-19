<script setup>
import SiteLayout from '../Layouts/SiteLayout.vue';
import HeroBanner from '../Components/HeroBanner.vue';
import ProductCategorySection from "../Components/ProductCategorySection.vue";
import TrustBar from "../Components/TrustBar.vue";
import ModelsGallery from "../Components/ModelsGallery.vue";
import AboutSection from "../Components/AboutSection.vue";

defineOptions({
    layout: SiteLayout,
});

const heroTitle = 'CORTINAS,\nPERSIANAS,\nTECIDOS &\nACESSÓRIOS.';

const modelos = [
    {
        id: 1,
        name: 'Cortina Ilhós',
        category: 'Cortinas',
        image: '/images/modelos/ilhos.png',
    },
    {
        id: 2,
        name: 'Cortina Prega Voil',
        category: 'Cortinas',
        image: '/images/modelos/prega-voil.png',
    },
    {
        id: 3,
        name: 'Persiana Especial',
        category: 'Persianas',
        image: '/images/modelos/persiana-especial.png',
    },
    {
        id: 4,
        name: 'Persiana Horizontal',
        category: 'Persianas',
        image: '/images/modelos/persiana-horizontal.jpg',
    },
    {
        id: 5,
        name: 'Almofadas',
        category: 'Acessórios',
        image: '/images/accesorios/almofadas.webp',
    },
    {
        id: 6,
        name: 'Deslizantes',
        category: 'Acessórios',
        image: '/images/accesorios/deslizantes.webp',
    },
    {
        id: 7,
        name: 'Entretelas',
        category: 'Acessórios',
        image: '/images/accesorios/entretelas.webp',
    },
    {
        id: 8,
        name: 'Ilhós',
        category: 'Acessórios',
        image: '/images/accesorios/ilhos.webp',
    },
    {
        id: 9,
        name: 'Kit Wave',
        category: 'Acessórios',
        image: '/images/accesorios/kitwave.webp',
    },
    {
        id: 10,
        name: 'Pingentes',
        category: 'Acessórios',
        image: '/images/accesorios/pingentes.webp',
    },
    {
        id: 11,
        name: 'Tecidos',
        category: 'Acessórios',
        image: '/images/accesorios/tecidos.webp',
    },
    {
        id: 12,
        name: 'Trilho Suiço',
        category: 'Acessórios',
        image: '/images/accesorios/trilhosuico.webp',
    },
    {
        id: 13,
        name: 'Varões',
        category: 'Acessórios',
        image: '/images/accesorios/varoes.webp',
    }
];
</script>

<template>
    <HeroBanner
        :title="heroTitle"
        subtitle="Tudo que seu ambiente precisa está aqui!"
        image="/images/hero-banner.webp"
        button-text="Conheça nossos modelos"
        button-link="#cortinas"
    />

    <ProductCategorySection
        :autoplay="3"
        id="cortinas"
        eyebrow="Aconchego e elegância"
        title="Cortinas que vestem o seu espaço"
        description="Tecidos escolhidos por você, acabamento feito com capricho e a medida exata da sua janela."
        :models="[
            {nome: 'Ilhós', icone: '/assets/icons/ilhos.svg'},
            {nome: 'Prega', icone: '/assets/icons/prega.svg'},
            {nome: 'Wave', icone: '/assets/icons/wave.svg'},
            {nome: 'Franzido', icone: '/assets/icons/franzido.svg'},
            {nome: 'Xale', icone: '/assets/icons/xale.svg'},
            {nome: 'Bandô', icone: '/assets/icons/bando.svg'},
        ]"
        :images="[
        {
            src: '/images/modelos/prega-voil.png',
            alt: 'Cortina clara em uma sala de estar',
            caption: 'Leveza e sofisticação',
        },
        {
            src: '/images/modelos/prega-voil.png',
            alt: 'Cortina decorativa em um ambiente moderno',
            caption: 'Elegância em cada detalhe',
        },
        {
            src: '/images/modelos/prega-voil.png',
            alt: 'Cortina instalada em uma janela',
            caption: 'Feita para o seu espaço',
        },
    ]"
    />

    <TrustBar/>

    <ProductCategorySection
        :autoplay="3"
        id="persianas"
        eyebrow="Luz e privacidade"
        title="Persianas que desenham a luz"
        description="Controle a entrada de luz, preserve sua privacidade e ganhe sofisticação com persianas sob medida."
        :models="[
            {nome: 'Horizontal', icone: '/assets/icons/horizontal.svg'},
            {nome: 'Rolô', icone: '/assets/icons/rolo.svg'},
            {nome: 'Romana', icone: '/assets/icons/romana.svg'},
            {nome: 'Painel', icone: '/assets/icons/painel.svg'},
            {nome: 'Vertical', icone: '/assets/icons/vertical.svg'},
        ]"
        :images="[
        {
            src: '/images/modelos/prega-voil.png',
            alt: 'Cortina clara em uma sala de estar',
            caption: 'Leveza e sofisticação',
        },
        {
            src: '/images/modelos/prega-voil.png',
            alt: 'Cortina decorativa em um ambiente moderno',
            caption: 'Elegância em cada detalhe',
        },
        {
            src: '/images/modelos/prega-voil.png',
            alt: 'Cortina instalada em uma janela',
            caption: 'Feita para o seu espaço',
        },
    ]"
    />

    <ModelsGallery
        id="modelos"
        :models="modelos"
        :autoplay="5"
        title="Um modelo para cada estilo"
        description="Conheça nossas cortinas, persianas e acessórios e descubra a que combina com o seu ambiente."
    />

    <AboutSection
        title="Do primeiro contato à instalação, com carinho"
        description="Cada projeto é feito sob medida, com atendimento próximo e atenção a cada detalhe, para o resultado ser exatamente o que você imaginou."
        image="/images/imagem_loja.png"
        image-alt="Ambiente decorado com cortinas da loja"
    />
</template>
