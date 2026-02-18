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
                            ? 'text-black'
                            : 'text-gray-300'
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
                                    ? 'text-black font-semibold'
                                    : 'text-gray-400'
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
        <div class="w-px bg-gray-200 shrink-0" />

        <!-- Right panel: detail content, fades on change -->
        <div class="flex-1 pl-8 flex flex-col justify-center">
            <Transition name="detail" mode="out-in">
                <div
                    v-if="currentDetail"
                    :key="focusedItem"
                    :class="
                        currentDetail.image_position === 'left' || currentDetail.image_position === 'right'
                            ? 'flex items-center gap-6'
                            : 'flex flex-col'
                    "
                    :style="
                        currentDetail.image_position === 'left'
                            ? 'flex-direction: row-reverse'
                            : currentDetail.image_position === 'right'
                            ? 'flex-direction: row'
                            : ''
                    "
                >
                    <img
                        v-if="currentDetail.image && currentDetail.image_position === 'top'"
                        :src="currentDetail.image"
                        :class="['object-contain mx-auto mb-6', currentDetail.imageClass || 'h-36']"
                    />
                    <p
                        class="text-base leading-relaxed"
                        :class="
                            currentDetail.image_position === 'left' || currentDetail.image_position === 'right'
                                ? 'flex-1'
                                : currentDetail.image_position === 'bottom' || !currentDetail.image_position
                                ? 'mb-6'
                                : ''
                        "
                        v-html="currentDetail.text"
                    />
                    <img
                        v-if="currentDetail.image && currentDetail.image_position !== 'top'"
                        :src="currentDetail.image"
                        :class="[
                            'object-contain',
                            currentDetail.image_position === 'left' || currentDetail.image_position === 'right' ? '' : 'mx-auto',
                            currentDetail.imageClass || 'h-36',
                        ]"
                    />
                </div>
            </Transition>
        </div>
    </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

const props = defineProps({
    initialCollapsed: {
        type: Boolean,
        default: false,
    },
});

const rootEl = ref(null);

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
            { id: "tauri", label: "Tauri" },
            { id: "dioxus", label: "Dioxus" },
        ],
    },
    {
        id: "blockchain",
        label: "Blockchain",
        items: [{ id: "solana", label: "Solana" }],
    },
    {
        id: "mobile",
        label: "Mobile Development",
        items: [],
    },
];

const details = {
    linux: {
        text: "<b>In December 2025 it was decided to promote Rust from experimental to a core part of the kernel.</b><br/><br/><b>This expands the core languages in the Linux kernel to C, assembly and Rust.</b>",
        image: "/images/slide6_img3.png",
    },
    android: {
        text: '<b>In Android 13, about 21% of all new native code is in Rust.</b><br/><b>1.5 million total lines of Rust code in AOSP</b><br/><br/><i>"A 1000x reduction in memory safety vulnerability density compared to Android\'s C and C++ code."</i><br/><br/><i>"With Rust changes having a 4x lower rollback rate and spending 25% less time in code review"</i>',
        image: "/images/android_logo.png",
    },
    embedded: {
        text: "<b>Rust is increasingly used in embedded systems</b> due to its zero-cost abstractions, memory safety without a runtime, and strong type system — making it ideal for microcontrollers and real-time systems.",
        image: "/images/rust_embedded.webp",
        image_position: "right",
    },
    codex: {
        text: "<b>OpenAI Codex CLI</b> is built with Rust, providing a fast and safe command-line interface for AI-powered code generation.",
        image: null,
    },
    turborepo: {
        text: "<b>Turborepo</b> rewrote its build system engine in Rust for dramatically improved performance and reliability in monorepo builds.",
        image: null,
    },
    ripgrep: {
        text: "<b>Ripgrep, fd, bat</b> and many other modern CLI tools are written in Rust — offering significant speed improvements and memory safety over their C counterparts.",
        image: null,
    },
    tauri: {
        text: "<b>Tauri</b> is a framework for building tiny, fast desktop apps using web frontends with a Rust backend — a lightweight alternative to Electron.",
        image: null,
    },
    dioxus: {
        text: "<b>Dioxus</b> is a Rust-native UI library inspired by React. It targets web, desktop, and mobile from a single codebase.",
        image: null,
    },
    solana: {
        text: "<b>Solana</b> is one of the fastest blockchain platforms, and its entire runtime and smart contract SDK are written in Rust.",
        image: null,
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
