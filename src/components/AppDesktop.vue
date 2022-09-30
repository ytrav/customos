<template>

    <div>
        <VueSelecto :selectByClick="true" :selectFromInside="true" :continueSelect="false"
            :toggleContinueSelect='"shift"' :hitRate="100" @select="onSelect" />
    </div>
</template>

<script>

import { VueSelecto } from "vue-selecto";
export default {
    name: 'AppDesktop',
    components: {
        VueSelecto,
    },
    methods: {
        // drag selection
        onSelect(e) {
            e.added.forEach(el => {
                el.classList.add("selected");
            });
            e.removed.forEach(el => {
                el.classList.remove("selected");
            });
        },
    }
}
</script>

<style lang="scss" scoped>
div {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    // background-color: antiquewhite;
}
.rCS179bs95 {
    z-index: 1;
    border-radius: 2px;
    border: 1px solid #52959F;
    background: #83b8c463;
    backdrop-filter: blur(6px);
    // transition: transform 0.1s;
}
</style>