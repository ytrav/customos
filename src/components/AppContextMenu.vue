<template>
    <div id="contextmenu" style="top: -300px; left: 0;">
        <span>Sort</span>
        <span @click="reloadPage">Refresh</span>
        <span @click="$emit('newFile')">New File</span>
        <span>New Folder</span>
        <span>Change Wallpaper</span>
    </div>
</template>

<script>
export default {
    methods: {
        reloadPage() {
            window.location.reload();
        },
    }
}
</script>

<style lang="scss" scoped>
div {
    display: flex;
    flex-direction: column;
    // align-items: flex-start;
    justify-content: center;
    // transform: translate3d(0, 0, 0);
    position: absolute;
    background-color: #B2C5CB;
    width: 170px;
    height: 200px;
    top: 0px;
    left: 0px;
    // transition: left 0.2s ease, top 0.2s ease;
    // transition: opacity 0.2s ease-out;
    // transition: height .2s ease;
    padding: 10px 0 10px 0;
    border-radius: 10px;
    z-index: 30;
    overflow: hidden;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
    user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
}

span {
    // width: 100%;
    text-align: left;
    padding: 10px;
    &:hover {
        background-color: #52959F;
        color: #fff;
    }
}
</style>