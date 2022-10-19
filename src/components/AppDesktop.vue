<template>
    <div id="desktop" @mousedown="$emit('deskClick')">
        <!-- <div @mousedown="$emit('trigger-hide-input')" v-if="inputTriggerVisible" id="inputCloseTrigger"></div> -->
        <VueSelecto :selectByClick="true" :selectFromInside="true" :continueSelect="false"
            :toggleContinueSelect='"ctrl"' :selectableTargets="['.selecto-area .icon']" :hitRate="0" :ratio="0"
            @select="onSelect" />
        <div class="elements selecto-area">
            <article class="icon" v-for="(icon, idx) in icons" :key="idx" :title="icon.name">
                <!-- <img :src="icon.img" :alt="alt"> -->
                <div class="imgplaceholder">
                    <svg viewBox="0 0 24 24">
                        <path
                            d="M13,9V3.5L18.5,9M6,2C4.89,2 4,2.89 4,4V20A2,2 0 0,0 6,22H18A2,2 0 0,0 20,20V8L14,2H6Z" />
                    </svg>
                </div>
                <label v-show="!icon.inputVisible" @dblclick="$emit('show-input', idx)" :for="icon.name">{{ icon.name
                }}.{{ icon.ext }}</label>
                <input :id="'input'+icon.name" @keyup.enter="$emit('deskClick')" @mousedown.stop :class="{inputVisible: icon.inputVisible}" type="text" :name="icon.name" v-model="icon.name">
            </article>
        </div>
        <div class="empty elements"></div>
    </div>
</template>

<script>

import { VueSelecto } from "vue-selecto";
export default {
    name: 'AppDesktop',
    props: {
        icons: Array,
    },
    data() {
        return {}
    },
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
#desktop {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    // z-index: 60;
    // background-color: antiquewhite;
}

.imgplaceholder {
    width: 50px;
    height: 50px;

    // background-color: #fff;
    svg {
        height: 100%;
        width: 100%;
        fill: #fff;
    }
}

.selecto-area {

    display: flex;
    flex-wrap: wrap;
    gap: 25px;
    padding: 25px;
    z-index: 10;
}

article {
    width: 100px;
    height: 75px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    border-radius: 5px;
    padding: 10px 0 0 0;

    label {
        word-break: break-all;
        // mix-blend-mode: exclusion;
        color: #fff;
        user-select: none;
        -webkit-user-select: none;
        -moz-user-select: none;
        text-shadow: 0px 4px 3px rgba(0, 0, 0, 0.4),
            0px 8px 13px rgba(0, 0, 0, 0.1),
            0px 18px 23px rgba(0, 0, 0, 0.1);

    }

    input {
        display: none;
        width: 100%;
        // transform: translateY(-10px);
        background-color: #52959faa;
        backdrop-filter: blur(3px);
        color: #fff;
        border: none;
        padding: 2px;
        text-align: center;
        font-size: 1em;
        border-radius: 5px;
        z-index: 101;

        &.inputVisible {
            display: block;
        }
    }
}



.selected {
    background-color: #52959f97;
    backdrop-filter: blur(2px);
}

.rCS179bs95 {
    z-index: 1;
    border-radius: 2px;
    border: 1px solid #52959F;
    background: #83b8c463;
    backdrop-filter: blur(2px);
    // transition: transform 0.1s;
}
</style>