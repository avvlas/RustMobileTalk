<template>
    <div class="uniffi-lift-lower flex justify-center items-center mt-8">
        <svg
            viewBox="0 0 750 320"
            width="750"
            aria-labelledby="uniffi-lift-lower-title"
            role="img"
        >
            <title id="uniffi-lift-lower-title">
                UniFFI lifting and lowering flow
            </title>
            <defs>
                <marker
                    id="uniffi-arr"
                    viewBox="0 0 10 10"
                    refX="8"
                    refY="5"
                    markerWidth="6"
                    markerHeight="6"
                    orient="auto-start-reverse"
                >
                    <path
                        d="M2 1L8 5L2 9"
                        fill="none"
                        stroke="#333"
                        stroke-width="1.5"
                        stroke-linecap="round"
                    />
                </marker>
                <marker
                    id="uniffi-arr-dim"
                    viewBox="0 0 10 10"
                    refX="8"
                    refY="5"
                    markerWidth="6"
                    markerHeight="6"
                    orient="auto-start-reverse"
                >
                    <path
                        d="M2 1L8 5L2 9"
                        fill="none"
                        stroke="#999"
                        stroke-width="1.5"
                        stroke-linecap="round"
                    />
                </marker>
            </defs>

            <g>
                <rect
                    x="20"
                    y="90"
                    width="90"
                    height="44"
                    rx="8"
                    fill="#f5f5f5"
                    stroke="#666"
                />
                <text x="65" y="117" text-anchor="middle" class="node-text">
                    App.kt
                </text>
            </g>

            <g>
                <rect
                    x="140"
                    y="40"
                    width="220"
                    height="240"
                    rx="14"
                    fill="#dae8fc"
                    fill-opacity="0.25"
                    stroke="#6c8ebf"
                    stroke-dasharray="4 4"
                />
                <text
                    x="250"
                    y="270"
                    text-anchor="middle"
                    class="region-label"
                    fill="#1a4480"
                >
                    generated-ffi.kt
                </text>
                <rect
                    x="170"
                    y="90"
                    width="160"
                    height="44"
                    rx="8"
                    fill="#dae8fc"
                    stroke="#6c8ebf"
                />
                <text x="250" y="117" text-anchor="middle" class="type-text">
                    kotlin.String
                </text>
                <line
                    x1="112"
                    y1="112"
                    x2="166"
                    y2="112"
                    stroke="#333"
                    stroke-width="1.5"
                    marker-end="url(#uniffi-arr)"
                />
            </g>

            <g>
                <rect
                    x="680"
                    y="90"
                    width="60"
                    height="44"
                    rx="8"
                    fill="#f5f5f5"
                    stroke="#666"
                />
                <text x="710" y="117" text-anchor="middle" class="node-text">
                    lib.rs
                </text>
            </g>

            <g v-click="[1, 2]">
                <line
                    class="mystery-line"
                    x1="332"
                    y1="112"
                    x2="676"
                    y2="112"
                    stroke="#999"
                    stroke-width="1.5"
                    marker-end="url(#uniffi-arr-dim)"
                />
                <g class="mystery-q">
                    <rect x="494" y="98" width="20" height="28" fill="#fff" />
                    <text
                        x="504"
                        y="119"
                        text-anchor="middle"
                        class="mystery-label"
                    >
                        ?
                    </text>
                </g>
            </g>

            <g v-click="2">
                <rect
                    x="170"
                    y="186"
                    width="160"
                    height="44"
                    rx="8"
                    fill="#dae8fc"
                    stroke="#6c8ebf"
                />
                <text x="250" y="213" text-anchor="middle" class="type-text">
                    RustBuffer.kt
                </text>
                <line
                    x1="250"
                    y1="136"
                    x2="250"
                    y2="182"
                    stroke="#333"
                    stroke-width="1.5"
                    marker-end="url(#uniffi-arr)"
                />
                <text x="262" y="163" class="edge-label">lower</text>
            </g>

            <g v-click="3">
                <rect
                    x="420"
                    y="40"
                    width="220"
                    height="240"
                    rx="14"
                    fill="#f8cecc"
                    fill-opacity="0.25"
                    stroke="#b85450"
                    stroke-dasharray="4 4"
                />
                <text
                    x="530"
                    y="270"
                    text-anchor="middle"
                    class="region-label"
                    fill="#7a2c2a"
                >
                    uniffi-rs generated
                </text>
                <rect
                    x="450"
                    y="186"
                    width="160"
                    height="44"
                    rx="8"
                    fill="#f8cecc"
                    stroke="#b85450"
                />
                <text x="530" y="213" text-anchor="middle" class="type-text">
                    RustBuffer
                </text>
                <line
                    x1="332"
                    y1="208"
                    x2="446"
                    y2="208"
                    stroke="#333"
                    stroke-width="1.5"
                    marker-end="url(#uniffi-arr)"
                />
                <text x="389" y="201" text-anchor="middle" class="edge-label">
                    JNA / C
                </text>
            </g>

            <g v-click="4">
                <rect
                    x="450"
                    y="90"
                    width="160"
                    height="44"
                    rx="8"
                    fill="#f8cecc"
                    stroke="#b85450"
                />
                <text x="530" y="117" text-anchor="middle" class="type-text">
                    std::String
                </text>
                <line
                    x1="530"
                    y1="182"
                    x2="530"
                    y2="136"
                    stroke="#333"
                    stroke-width="1.5"
                    marker-end="url(#uniffi-arr)"
                />
                <text x="542" y="163" class="edge-label">lift</text>
            </g>

            <g v-click="5">
                <line
                    x1="612"
                    y1="112"
                    x2="676"
                    y2="112"
                    stroke="#333"
                    stroke-width="1.5"
                    marker-end="url(#uniffi-arr)"
                />
            </g>
        </svg>
    </div>
</template>

<style scoped>
.uniffi-lift-lower svg {
    max-width: 100%;
    height: auto;
}

.node-text {
    font-family: sans-serif;
    font-size: 14px;
    font-weight: 500;
}

.type-text {
    font-family: "JetBrains Mono", "Fira Code", monospace;
    font-size: 13px;
    font-weight: 500;
}

.edge-label {
    fill: #333;
    font-family: sans-serif;
    font-size: 12px;
}

.region-label {
    font-family: sans-serif;
    font-size: 11px;
    font-style: italic;
}

.mystery-label {
    fill: #999;
    font-family: sans-serif;
    font-size: 18px;
    font-weight: 600;
}

:deep(.slidev-vclick-target) {
    transition: opacity 0.4s ease-in-out;
}

@keyframes draw-line {
    from {
        stroke-dashoffset: 360;
    }

    to {
        stroke-dashoffset: 0;
    }
}

@keyframes fade-in-q {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}

.mystery-line {
    animation: draw-line 1.2s ease-out forwards;
    stroke-dasharray: 5 5;
}

.mystery-q {
    animation: fade-in-q 0.4s ease-out 1s forwards;
    opacity: 0;
}
</style>
