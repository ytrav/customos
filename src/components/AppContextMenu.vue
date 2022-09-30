<template>
    <div id="contextmenu" style="opacity: 0;">
        <span>Sort</span>
        <span>Refresh</span>
        <span>New File</span>
        <span>New Folder</span>
    </div>
</template>

<style lang="scss" scoped>
div {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    // transform: translate3d(0, 0, 0);
    position: absolute;
    background-color: #B2C5CB;
    width: 100px;
    height: 100px;
}
</style>