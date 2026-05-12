<script setup>
const imageSources = {
    6: [
        {
            label: "StackOverflow Dev Survey 2025",
            href: "https://survey.stackoverflow.co/2025/",
        },
    ],
    7: [
        {
            label: "Windows Latest",
            href: "https://www.windowslatest.com/2025/12/24/microsoft-confirms-eliminate-c-and-c-plan-translate-code-to-rust-using-ai-as-windows-11-adopts-rust-and-webview2/",
        },
    ],
    8: [
        {
            label: "CaffeinatedCoder @ Medium",
            href: "https://caffeinatedcoder.medium.com/we-rewrote-everything-in-rust-then-the-seniors-quit-5cd202c71376",
        },
    ],
    22: [
        {
            label: "UniFFI Docs",
            href: "https://mozilla.github.io/uniffi-rs/latest/",
        },
    ],
    34: [
        {
            label: "Crux overview",
            href: "https://redbadger.github.io/crux/",
        },
    ],
    35: [
        {
            label: "Crux overview",
            href: "https://redbadger.github.io/crux/",
        },
    ],
    36: [
        {
            label: "Crux overview",
            href: "https://redbadger.github.io/crux/",
        },
    ],
    37: [
        {
            label: "Facet-generate pull #48",
            href: "https://github.com/redbadger/facet-generate/pull/48",
        },
        {
            label: "Serde-reflection pull #86",
            href: "https://github.com/zefchain/serde-reflection/pull/86",
        },
    ],
    38: [
        {
            label: "Crux pull #485",
            href: "https://github.com/redbadger/crux/pull/485",
        },
    ],
    39: [
        {
            label: "Photoroom",
            href: "https://www.photoroom.com/",
        },
    ],
    40: [
        {
            label: "Proton",
            href: "https://proton.me/",
        },
    ],
    43: [
        {
            label: "Dioxus Labs",
            href: "https://dioxuslabs.com/",
        },
    ],
    54: [
        {
            label: "Telegram",
            href: "https://t.me/avvlased",
        },
    ],
};
</script>

<template>
    <div v-if="imageSources[$nav.currentSlideNo]?.length" class="image-sources">
        <span v-if="![37, 38].includes($nav.currentSlideNo)" class="prefix"
            >Источник:</span
        >
        <span
            v-for="source in imageSources[$nav.currentSlideNo]"
            :key="source.href"
            class="source-item"
        >
            <a :href="source.href" target="_blank" rel="noreferrer noopener">
                {{ source.label }}
            </a>
        </span>
    </div>

    <footer v-if="$nav.currentSlideNo > 1" class="slide-number">
        {{ $nav.currentSlideNo }} / {{ $nav.total }}
    </footer>
</template>

<style scoped>
.image-sources {
    position: fixed;
    left: 1rem;
    right: 5rem;
    bottom: 0.55rem;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 0.35rem;
    width: fit-content;
    max-width: min(70vw, 58rem);
    padding: 0.2rem 0.45rem;
    font-size: 0.62rem;
    line-height: 1.2;
    color: #111827;
    background: rgba(255, 255, 255, 0.85);
    border-radius: 0.4rem;
    z-index: 100;
}

.prefix {
    opacity: 0.7;
}

.source-item + .source-item::before {
    content: "·";
    margin-right: 0.35rem;
    opacity: 0.45;
}

.image-sources a {
    color: inherit;
    text-decoration: underline;
    pointer-events: auto;
}

.slide-number {
    position: fixed;
    right: 1.5rem;
    bottom: 0.75rem;
    font-size: 0.75rem;
    opacity: 0.5;
    pointer-events: none;
    z-index: 100;
}
</style>
