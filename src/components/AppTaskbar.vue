<template>
    <footer>
        <div id="taskbar-icons">
            <div title="Start Menu" class="start-menu">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                    <path d="M3,6H21V8H3V6M3,11H21V13H3V11M3,16H21V18H3V16Z" />
                </svg>
            </div>
            <div class="taskbar-icon" v-for="(app, index) in apps" :key="index" :title="app.title"
                @click="toggleApp(app.appName, app.isOpen, app.isHidden)">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                    <path :d="app.svgPath" />
                </svg>
            </div>
        </div>
        <div id="control-panel"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                <path
                    d="M14,3.23V5.29C16.89,6.15 19,8.83 19,12C19,15.17 16.89,17.84 14,18.7V20.77C18,19.86 21,16.28 21,12C21,7.72 18,4.14 14,3.23M16.5,12C16.5,10.23 15.5,8.71 14,7.97V16C15.5,15.29 16.5,13.76 16.5,12M3,9V15H7L12,20V4L7,9H3Z" />
            </svg><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                <path
                    d="M12,8A4,4 0 0,0 8,12A4,4 0 0,0 12,16A4,4 0 0,0 16,12A4,4 0 0,0 12,8M12,18A6,6 0 0,1 6,12A6,6 0 0,1 12,6A6,6 0 0,1 18,12A6,6 0 0,1 12,18M20,8.69V4H15.31L12,0.69L8.69,4H4V8.69L0.69,12L4,15.31V20H8.69L12,23.31L15.31,20H20V15.31L23.31,12L20,8.69Z" />
            </svg>
            <div class="divider"></div><svg v-if="wifi === 'good'" xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24">
                <path
                    d="M12,3C7.79,3 3.7,4.41 0.38,7C4.41,12.06 7.89,16.37 12,21.5C16.08,16.42 20.24,11.24 23.65,7C20.32,4.41 16.22,3 12,3Z" />
            </svg><svg v-else-if="wifi === 'medium'" viewBox="0 0 24 24">
                <path
                    d="M12,3C7.79,3 3.7,4.41 0.38,7C4.41,12.06 7.89,16.37 12,21.5C16.08,16.42 20.24,11.24 23.65,7C20.32,4.41 16.22,3 12,3M12,5C15.07,5 18.09,5.86 20.71,7.45L18.77,9.88C17.26,9 14.88,8 12,8C9,8 6.68,9 5.21,9.84L3.27,7.44C5.91,5.85 8.93,5 12,5Z" />
            </svg><svg v-else-if="wifi === 'bad'" viewBox="0 0 24 24">
                <path
                    d="M12,3C7.79,3 3.7,4.41 0.38,7C4.41,12.06 7.89,16.37 12,21.5C16.08,16.42 20.24,11.24 23.65,7C20.32,4.41 16.22,3 12,3M12,5C15.07,5 18.09,5.86 20.71,7.45L15.61,13.81C14.5,13.28 13.25,13 12,13C10.75,13 9.5,13.28 8.39,13.8L3.27,7.44C5.91,5.85 8.93,5 12,5Z" />
            </svg>
            <span id="time">{{ time }}</span>
        </div>
    </footer>
</template>

<script>
export default {
    name: 'AppTaskbar',
    props: {
        apps: {
            type: Array,
        }
    },
    data() {
        return {
            wifi: 'good',
            time: '00:00'
        }
    },
    methods: {
        changeWifiSpeed() {
            var set = this;
            setInterval(function () {
                let number = Math.floor(Math.random() * 11);
                if (number <= 10 && number >= 6) {
                    // alert('good');
                    set.wifi = 'good';
                } else if (number < 6 && number >= 3) {
                    // alert('medium');
                    set.wifi = 'medium';
                } else if (number < 3 && number >= 0) {
                    // alert('bad');
                    set.wifi = 'medium';
                    setTimeout(function () {
                        set.wifi = 'bad';
                    }, 2000);
                    setTimeout(function () {
                        set.wifi = 'medium';
                    }, 6000);
                }
            }, Math.floor(Math.random() * 10000) + 7000);
        },
        setTime() {
            var set = this;
            setInterval(function () {
                let time = new Date;
                let hours = time.getHours();
                if (hours <= 9) {
                    hours = `0${hours}`
                }
                let minutes = time.getMinutes();
                if (minutes <= 9) {
                    minutes = `0${minutes}`
                }
                set.time = `${hours}:${minutes}`;
            }, 2000)
        },
        toggleApp(name, opened, hidden) {
            // console.log(`name is ${name}, opened is ${opened} and hidden is ${hidden}`);
            if (opened === false) {
                switch (name) {
                    case 'numbers':
                        this.$emit('open', 0);
                        console.log('open num');
                        break;
                    case 'text':
                        this.$emit('open', 1);
                        console.log('open text');
                        break;
                    default:
                        console.log('unknown value');
                        break;
                }
                // console.log(`emitted open`);
                return;
            }
            if (opened === true && hidden === false) {
                switch (name) {
                    case 'numbers':
                        this.$emit('hide', 0);
                        console.log('hide num');
                        break;
                    case 'text':
                        this.$emit('hide', 1);
                        console.log('hide text');
                        break;
                    default:
                        console.log('unknown value');
                        break;
                }
                // console.log(`emitted hide`);
                return;
            }
            if (opened === true && hidden === true) {
                switch (name) {
                    case 'numbers':
                        this.$emit('show', 0);
                        console.log('show num');
                        break;
                    case 'text':
                        this.$emit('show', 1);
                        console.log('show text');
                        break;
                    default:
                        console.log('unknown value');
                        break;
                }
                // console.log(`emitted show`);
                return;
            }
        }
    },
    mounted() {
        let time = new Date;
        let hours = time.getHours();
        if (hours <= 9) {
            hours = `0${hours}`
        }
        let minutes = time.getMinutes();
        if (minutes <= 9) {
            minutes = `0${minutes}`
        }
        this.time = `${hours}:${minutes}`;
        this.changeWifiSpeed();
        this.setTime();
    },
}
</script>

<style lang="scss" scoped>
footer {
    position: absolute;
    bottom: 0;
    right: 0;
    left: 0;
    height: 40px;
    // background-color: gray;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    z-index: 6;
    // padding: 0 10px 0 10px;
}

#taskbar-icons {
    display: flex;
    border-radius: 0 15px 0 0;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
    overflow: hidden;
}

.start-menu,
.taskbar-icon,
#control-panel {
    height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: #52959fa4;
    backdrop-filter: blur(4px);
}

.start-menu,
.taskbar-icon {
    width: 65px;

    &:hover {
        background-color: rgb(221, 221, 221);

        svg {
            fill: gray;
        }
    }

    &:active {
        filter: brightness(0.9);
    }
}

// .taskbar-icon:nth-last-child(1) {
//     border-radius: 0 15px 0 0;
// }

svg {
    height: 30px;
    width: 30px;
    min-height: 30px;
    min-width: 30px;
    fill: #fff;
}

#control-panel {
    position: absolute;
    right: 0;
    bottom: 0;
    // left: 0;
    border-radius: 15px 0 0 0;
    padding: 0 15px 0 15px;
    // justify-content: space-between;
    gap: 15px;
    box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;

    svg {
        width: 25px;
        height: 25px;
        min-width: 25px;
        min-height: 25px;
    }
}

.divider {
    height: 100%;
    width: 2px;
    background-color: #c2c2c280;
}

#time {
    color: #fff;
    font-size: 1.4em;
    font-weight: 500;
    width: 60px;
    user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
}
</style>
