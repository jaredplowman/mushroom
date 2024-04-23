<script setup lang="ts">
import OpenSeadragon from "openseadragon";

let viewer: OpenSeadragon.Viewer;

onMounted(() => {
    viewer = OpenSeadragon({
        id: "openseadragon1",
        preserveViewport: true,
        tileSources: [
            {
                type: "image",
                url: "/img/bed1.png",
                // overlays: [
                //     {
                //         id: "example-overlay",
                //         x: 0.175,
                //         y: 0.167,
                //         width: 0.02,
                //         height: 0.02,
                //         className: "highlight",
                //     },
                // ],
            },
        ],
        visibilityRatio: 1.0,
        constrainDuringPan: true,
        maxZoomPixelRatio: 5,
        minZoomImageRatio: 1,
        springStiffness: 3,
        animationTime: 0.2,
    });

    viewer.addOnceHandler("open", function (event) {
        // MouseTracker is required for links to function in overlays
        new OpenSeadragon.MouseTracker({
            element: "example-overlay",
            enterHandler: function (event) {
                container.value?.dispatchEvent(
                    new CustomEvent<PointerEvent>("mpuOpen", {
                        detail: event.originalEvent as PointerEvent,
                    })
                );

                console.log("enterHandler", event);
            },
            leaveHandler: function (event) {
                container.value?.dispatchEvent(new Event("mpuClose"));
            },
        });
    });

    if (process.client) {
        container.value?.addEventListener("mpuOpen", ((e: CustomEvent<PointerEvent>) => {
            if (!popupRef.value || showPopup.value) {
                return;
            }

            popupRef.value.style.position = "absolute";
            popupRef.value.style.left = e.detail.clientX + "px";
            popupRef.value.style.top = e.detail.clientY + "px";
            showPopup.value = true;
        }) as EventListener);

        container.value?.addEventListener("mpuClose", (() => {
            showPopup.value = false;
        }) as EventListener);
    }
});

function showHighlightHandler() {
    viewer.addOverlay({
        id: "example-overlay",
        x: 0.175,
        y: 0.167,
        width: 0.02,
        height: 0.02,
        className: "highlight",
    });
}

function hideHighlightHandler() {
    viewer.removeOverlay("example-overlay");
}

const showPopup = ref(false);
const popupRef = ref<HTMLDivElement>();
const container = ref<HTMLDivElement>();
</script>

<template>
    <div class="w-100 flex-column justify-content-center p-4" ref="container">
        <div class="flex gap-2 pb-4">
            <UButton @click="showHighlightHandler">Show Highlight</UButton>
            <UButton @click="hideHighlightHandler">Hide Highlight</UButton>
        </div>
        <div id="openseadragon1" />
        <div id="example-overlay" v-show="false" />
        <div
            v-show="showPopup"
            ref="popupRef"
            class="flex-col p-2 rounded-md bg-slate-500"
        >
            <div>Mushrooms!!! Everywhere</div>
            <div>Mushrooms!!! Everywhere</div>
            <div>Mushrooms!!! Everywhere</div>
        </div>
    </div>
</template>

<style lang="scss">
#openseadragon1 {
    width: 1262px;
    height: 471px;
}

#example-overlay {
    border: 2px solid fuchsia;
    background-color: fuchsia;
    border-radius: 100%;
    opacity: 0.55;
}
</style>
