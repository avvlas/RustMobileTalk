<template>
    <div ref="rootEl" class="flex h-full gap-0">
        <!-- Left column: headings + items -->
        <div class="flex flex-col justify-center gap-1 w-64 shrink-0 pr-6">
            <template v-for="cat in categories" :key="cat.id">
                <!-- Category heading -->
                <div
                    class="cursor-pointer select-none transition-all duration-200 text-lg font-bold py-1"
                    :class="
                        focusedCategory === cat.id || focusedCategory === null
                            ? ''
                            : 'text-gray-300 dark:text-gray-600'
                    "
                    @click="selectCategory(cat.id)"
                >
                    {{ cat.label }}
                </div>

                <!-- Items under focused category — animated accordion -->
                <Transition
                    name="accordion"
                    @enter="onEnter"
                    @after-enter="onAfterEnter"
                    @leave="onLeave"
                >
                    <div
                        v-if="focusedCategory === cat.id"
                        class="overflow-hidden"
                    >
                        <div
                            v-for="item in cat.items"
                            :key="item.id"
                            class="cursor-pointer select-none pl-4 py-0.5 text-base transition-colors duration-150"
                            :class="
                                focusedItem === item.id
                                    ? 'font-semibold'
                                    : 'text-gray-400 dark:text-gray-500'
                            "
                            @click.stop="selectItem(item.id)"
                        >
                            {{ item.label }}
                        </div>
                    </div>
                </Transition>
            </template>
        </div>

        <!-- Divider -->
        <div class="w-px bg-gray-200 dark:bg-gray-700 shrink-0" />

        <!-- Right panel: detail content, fades on change -->
        <div class="flex-1 pl-8 flex flex-col justify-center">
            <Transition name="detail" mode="out-in">
                <div
                    v-if="currentDetail"
                    :key="focusedItem"
                    :class="
                        currentDetail.images
                            ? 'flex items-center justify-center'
                            : currentDetail.image_position === 'left' ||
                                currentDetail.image_position === 'right'
                              ? 'flex items-center gap-6'
                              : 'flex flex-col'
                    "
                    :style="
                        !currentDetail.images &&
                        currentDetail.image_position === 'left'
                            ? 'flex-direction: row-reverse'
                            : !currentDetail.images &&
                                currentDetail.image_position === 'right'
                              ? 'flex-direction: row'
                              : ''
                    "
                >
                    <template v-if="currentDetail.images">
                        <div class="flex flex-col items-center gap-4">
                            <template v-if="currentDetail.images.length === 3">
                                <img
                                    :src="assetUrl(currentDetail.images[0])"
                                    :class="[
                                        'object-contain',
                                        currentDetail.imageClass || 'h-36',
                                    ]"
                                />
                                <div class="flex gap-8">
                                    <img
                                        :src="assetUrl(currentDetail.images[1])"
                                        :class="[
                                            'object-contain',
                                            currentDetail.imageClass || 'h-36',
                                        ]"
                                    />
                                    <img
                                        :src="assetUrl(currentDetail.images[2])"
                                        :class="[
                                            'object-contain',
                                            currentDetail.imageClass || 'h-36',
                                        ]"
                                    />
                                </div>
                            </template>
                            <template v-else>
                                <div class="flex gap-8">
                                    <img
                                        v-for="(
                                            imgSrc, idx
                                        ) in currentDetail.images"
                                        :key="idx"
                                        :src="assetUrl(imgSrc)"
                                        :class="[
                                            'object-contain',
                                            currentDetail.imageClass || 'h-36',
                                        ]"
                                    />
                                </div>
                            </template>
                        </div>
                    </template>
                    <template v-else>
                        <img
                            v-if="
                                currentDetail.image &&
                                (currentDetail.image_position === 'top' ||
                                    !currentDetail.image_position)
                            "
                            :src="assetUrl(currentDetail.image)"
                            :class="[
                                'object-contain mx-auto mb-6',
                                currentDetail.imageClass || 'h-36',
                            ]"
                        />
                        <p
                            class="text-base leading-relaxed"
                            :class="
                                currentDetail.image_position === 'left' ||
                                currentDetail.image_position === 'right'
                                    ? 'flex-1'
                                    : currentDetail.image_position === 'bottom'
                                      ? 'mb-6'
                                      : ''
                            "
                            v-html="currentDetail.text"
                        />
                        <img
                            v-if="
                                currentDetail.image &&
                                currentDetail.image_position !== 'top' &&
                                currentDetail.image_position
                            "
                            :src="assetUrl(currentDetail.image)"
                            :class="[
                                'object-contain',
                                currentDetail.image_position === 'left' ||
                                currentDetail.image_position === 'right'
                                    ? ''
                                    : 'mx-auto',
                                currentDetail.imageClass || 'h-36',
                            ]"
                        />
                    </template>
                </div>
            </Transition>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";
import androidLogo from "../public/images/android_logo.png";
import batSocial from "../public/images/bat_social.png";
import codexSocial from "../public/images/codex_social.png";
import fdSocial from "../public/images/fd_social.png";
import fuelSocial from "../public/images/fuel_social.png";
import opencodeImage from "../public/images/opencode.png";
import protonSuite from "../public/images/proton_suite.webp";
import ripgrepSocial from "../public/images/ripgrep_social.png";
import rustEmbedded from "../public/images/rust_embedded.webp";
import slide6Image from "../public/images/slide6_img3.png";
import slide9Image6 from "../public/images/slide9_img6.png";
import slide9Image7 from "../public/images/slide9_img7.png";
import slide10Image8 from "../public/images/slide10_img8.png";
import slide11Image11 from "../public/images/slide11_img11.png";
import solanaSocial from "../public/images/solana_social.png";
import tauriSocial from "../public/images/tauri_social.png";
import turborepoSocial from "../public/images/turborepo_social.png";
import zedSocial from "../public/images/zed_social.png";

const props = defineProps({
    initialCollapsed: {
        type: Boolean,
        default: false,
    },
});

const rootEl = ref(null);

function assetUrl(path) {
    return path || "";
}

function isActive() {
    if (!rootEl.value) return false;
    const rect = rootEl.value.getBoundingClientRect();
    return (
        rect.width > 0 &&
        rect.height > 0 &&
        rect.left >= 0 &&
        rect.left < window.innerWidth
    );
}

const categories = [
    {
        id: "systems",
        label: "Systems Programming",
        items: [
            { id: "linux", label: "Linux Kernel" },
            { id: "android", label: "Android OS" },
            { id: "embedded", label: "Embedded" },
        ],
    },
    {
        id: "cli",
        label: "CLI Tools",
        items: [
            { id: "codex", label: "Codex" },
            { id: "turborepo", label: "Turborepo" },
            { id: "ripgrep", label: "Ripgrep, fd, bat, …" },
        ],
    },
    {
        id: "desktop",
        label: "Desktop & Web Apps",
        items: [
            { id: "zed", label: "Zed IDE" },
            { id: "tauri", label: "Tauri" },
            { id: "opencode", label: "OpenCode" },
        ],
    },
    {
        id: "blockchain",
        label: "Blockchain & Crypto",
        items: [
            { id: "fuel", label: "Fuel" },
            { id: "solana", label: "Solana" },
        ],
    },
    {
        id: "mobile",
        label: "Mobile Development",
        items: [
            { id: "elementx", label: "ElementX" },
            { id: "protonpass", label: "Proton Suite" },
            { id: "firefox_mobile", label: "Firefox" },
        ],
    },
];

const details = {
    linux: {
        text: "<b>In December 2025 it was decided to promote Rust from experimental to a core part of the kernel.</b><br/><br/><b>This expands the core languages in the Linux kernel to C, assembly and Rust.</b>",
        image: slide6Image,
        image_position: "top",
        imageClass: "h-36",
    },
    android: {
        text: '<b>In Android 13, about 21% of all new native code is in Rust.</b><br/><b>1.5 million total lines of Rust code in AOSP</b><br/><br/><i>"A 1000x reduction in memory safety vulnerability density compared to Android\'s C and C++ code.<br/>With Rust changes having a 4x lower rollback rate and spending 25% less time in code review"</i>',
        image: androidLogo,
        image_position: "top",
        imageClass: "h-24",
    },
    embedded: {
        text: null,
        image: rustEmbedded,
        image_position: "top",
        imageClass: "h-56",
    },
    codex: {
        text: null,
        image: codexSocial,
        imageClass: "h-44",
    },
    turborepo: {
        text: null,
        image: turborepoSocial,
        imageClass: "h-44",
    },
    ripgrep: {
        text: null,
        images: [ripgrepSocial, fdSocial, batSocial],
        imageClass: "h-36",
    },
    opencode: {
        text: null,
        image: opencodeImage,
        imageClass: "scale-100",
    },
    zed: {
        text: null,
        image: zedSocial,
        imageClass: "h-44",
    },
    tauri: {
        text: null,
        image: tauriSocial,
        imageClass: "h-44",
    },
    fuel: {
        image: fuelSocial,
        imageClass: "h-44",
    },
    solana: {
        text: null,
        image: solanaSocial,
        imageClass: "h-44",
    },
    elementx: {
        text: "<b>ElementX</b> — мессенджер на протоколе Matrix.<br/>Matrix Rust SDK.",
        images: [slide9Image6, slide9Image7],
        imageClass: "h-72",
    },
    protonpass: {
        text: "<b>Proton Suite</b> — менеджер паролей (а также email, vpn, cloud storage,..).<br/>Common library на Rust.",
        images: [slide10Image8, protonSuite],
        imageClass: "h-72",
    },
    firefox_mobile: {
        text: null,
        image: slide11Image11,
        image_position: "right",
        imageClass: "scale-60",
    },
};

// Static flat list: every category heading, then every item, in order
const flatList = (() => {
    const list = [];
    for (const cat of categories) {
        list.push({ type: "category", id: cat.id });
        for (const item of cat.items) {
            list.push({ type: "item", id: item.id, catId: cat.id });
        }
    }
    return list;
})();

// Single pointer into flatList
// 0 = first category (collapsed/all-headings state), 1 = first item (linux)
const focusIdx = ref(props.initialCollapsed ? 0 : 1);

const focusedCategory = computed(() => {
    const entry = flatList[focusIdx.value];
    // When on a category entry, no category is expanded (all collapsed)
    if (entry.type === "category") return null;
    return entry.catId;
});

const focusedItem = computed(() => {
    const entry = flatList[focusIdx.value];
    return entry.type === "item" ? entry.id : null;
});

function applyIdx(idx) {
    focusIdx.value = idx;
}

function selectCategory(id) {
    const idx = flatList.findIndex((e) => e.type === "item" && e.catId === id);
    if (idx !== -1) applyIdx(idx);
}

function selectItem(id) {
    const idx = flatList.findIndex((e) => e.type === "item" && e.id === id);
    if (idx !== -1) applyIdx(idx);
}

// Returns true if the key was consumed, false if the slide should handle it
function moveDown() {
    for (let i = focusIdx.value + 1; i < flatList.length; i++) {
        if (flatList[i].type === "item") {
            focusIdx.value = i;
            return true;
        }
    }
    return false; // at end → let slide navigate forward
}

function moveUp() {
    for (let i = focusIdx.value - 1; i >= 0; i--) {
        if (flatList[i].type === "item") {
            focusIdx.value = i;
            return true;
        }
    }
    return false; // at start → let slide navigate back
}

function onKeyDown(e) {
    if (!isActive()) return;
    if (e.key === "ArrowDown" || e.key === "ArrowRight") {
        if (moveDown()) e.stopPropagation();
    } else if (e.key === "ArrowUp" || e.key === "ArrowLeft") {
        if (moveUp()) e.stopPropagation();
    } else if (e.key === " " || e.key === "Spacebar") {
        const moved = e.shiftKey ? moveUp() : moveDown();
        if (moved) {
            e.preventDefault();
            e.stopPropagation();
        }
    }
}

onMounted(() => {
    window.addEventListener("keydown", onKeyDown, true);
});

onUnmounted(() => {
    window.removeEventListener("keydown", onKeyDown, true);
});

const currentDetail = computed(() =>
    focusedItem.value ? (details[focusedItem.value] ?? null) : null,
);

// Accordion animation helpers — animate height from 0 to natural height
function onEnter(el) {
    el.style.height = "0";
    el.style.opacity = "0";
    requestAnimationFrame(() => {
        el.style.transition = "height 0.25s ease, opacity 0.25s ease";
        el.style.height = el.scrollHeight + "px";
        el.style.opacity = "1";
    });
}

function onAfterEnter(el) {
    el.style.height = "";
    el.style.transition = "";
    el.style.opacity = "";
}

function onLeave(el) {
    el.style.height = el.scrollHeight + "px";
    el.style.opacity = "1";
    requestAnimationFrame(() => {
        el.style.transition = "height 0.2s ease, opacity 0.2s ease";
        el.style.height = "0";
        el.style.opacity = "0";
    });
}
</script>

<style scoped>
/* Right panel: fade + slight upward slide */
.detail-enter-active,
.detail-leave-active {
    transition:
        opacity 0.2s ease,
        transform 0.2s ease;
}
.detail-enter-from {
    opacity: 0;
    transform: translateY(8px);
}
.detail-leave-to {
    opacity: 0;
    transform: translateY(-8px);
}
</style>
