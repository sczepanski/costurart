<script setup>
defineProps({
    variant: {
        type: String,
        default: 'solid',
        validator: (value) => ['solid', 'outline'].includes(value),
    },
    href: {
        type: String,
        default: null,
    },
    external: {
        type: Boolean,
        default: false,
    },
    type: {
        type: String,
        default: 'button',
    },
    disabled: {
        type: Boolean,
        default: false,
    },
    size: {
        type: String,
        default: 'lg',
        validator: (value) => ['lg', 'sm', 'xs'].includes(value),
    },
    rounded: {
        type: Boolean,
        default: false,
    },
});
</script>

<template>
    <!-- Renderiza um link quando href é informado -->
    <a
        v-if="href"
        :href="href"
        :target="external ? '_blank' : undefined"
        :rel="external ? 'noopener noreferrer' : undefined"
        :class="[
        'inline-flex items-center justify-center gap-2 font-semibold text-white cursor-pointer',
        'transition-[background-position,box-shadow,transform,filter,scale] duration-300 ease-out active:scale-95',
        variant === 'solid'
            ? 'button-solid hover:brightness-125 shadow-lg shadow-primary-600/0 hover:shadow-lg hover:shadow-primary-600/30'
            : 'button-outline',
        {
            lg: 'px-6 py-3',
            sm: 'px-4 py-2 text-sm',
        }[size],
        rounded ? 'rounded-full' : 'rounded-xl',
    ]"
    >
        <slot/>
    </a>

    <!-- Renderiza um botão quando não há href -->
    <button
        v-else
        :type="type"
        :disabled="disabled"
        :class="[
            'inline-flex items-center justify-center gap-2 font-semibold text-white cursor-pointer',
            'transition-[background-position,box-shadow,transform,filter,scale] duration-300 ease-out active:scale-95',
            variant === 'solid'
                ? 'button-solid hover:brightness-125 shadow-lg shadow-primary-600/0 hover:shadow-lg hover:shadow-primary-600/30'
                : 'button-outline',
            {
                lg: 'px-6 py-3',
                sm: 'px-4 py-2 text-sm',
            }[size],
            rounded ? 'rounded-full' : 'rounded-lg',
        ]"
    >
        <slot/>
    </button>
</template>
