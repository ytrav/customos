<template>
  <div id="app" @mousemove="mouseMoved($event)">
    <transition @contextmenu.prevent="handler" name="context">
      <AppContextMenu @contextmenu.native="nothing" v-show="contextVisible" />
    </transition>
    <AppDesktop @deskClick="hideContext" @click="hideContext" @contextmenu.native="handler($event)" />
    <modal :class="{ focused: app.inFocus}" :transition="app.transition" :id="'modal-'+index"
      styles="pointer-events: auto; border-radius: 15px;" v-for="(app, index) in taskbarApps" :key="index"
      :name="app.appName" :focusTrap="false" :minWidth="400" :minHeight="200" draggable=".window-header"
      :resizable="true" :adaptive="true" :clickToClose="false" :reset="!app.keepPos">
      <header @mousedown="focus(index)" class="window-header">
        <span>{{ app.title }}</span>
        <div class="window-control">
          <div class="hide" @click="hide(index)">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path d="M19,13H5V11H19V13Z" />
            </svg>
          </div>
          <div class="expand">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path
                d="M18,18H6V6H18M18,4H6A2,2 0 0,0 4,6V18A2,2 0 0,0 6,20H18A2,2 0 0,0 20,18V6C20,4.89 19.1,4 18,4Z" />
            </svg>
          </div>
          <div class="close" @click="close(index)">
            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
              <path
                d="M19,6.41L17.59,5L12,10.59L6.41,5L5,6.41L10.59,12L5,17.59L6.41,19L12,13.41L17.59,19L19,17.59L13.41,12L19,6.41Z" />
            </svg>
          </div>
        </div>
      </header>
      <div @mousedown="focus(index)" class="content">{{ app.content }}</div>
    </modal>
    <AppTaskbar @deskClick="hideContext" @mousedown="hideContext" @contextmenu.native="nothing" :apps="taskbarApps" @open="open($event)"
      @hide="hide($event)" @show="show($event)" />

  </div>

</template>

<script>
// import VModal from 'vue-js-modal'
import AppTaskbar from './components/AppTaskbar.vue';
import AppDesktop from './components/AppDesktop.vue';
import AppContextMenu from './components/AppContextMenu.vue';

export default {
  name: 'App',
  data() {
    return {
      document: 'document',
      hidden: false,
      cursorX: 0,
      cursorY: 0,
      contextVisible: false,
      taskbarApps: [
        {
          title: 'Calculator',
          appName: 'numbers',
          isOpen: false,
          isHidden: false,
          keepPos: false,
          inFocus: false,
          transition: 'openClose',
          svgPath: 'M19 3H5C3.9 3 3 3.9 3 5V19C3 20.1 3.9 21 5 21H19C20.1 21 21 20.1 21 19V5C21 3.9 20.1 3 19 3M19 19H5V5H19V19M6.2 7.7H11.2V9.2H6.2V7.7M13 15.8H18V17.3H13V15.8M13 13.2H18V14.7H13V13.2M8 18H9.5V16H11.5V14.5H9.5V12.5H8V14.5H6V16H8V18M14.1 10.9L15.5 9.5L16.9 10.9L18 9.9L16.6 8.5L18 7.1L16.9 6L15.5 7.4L14.1 6L13 7.1L14.4 8.5L13 9.9L14.1 10.9Z',
          content: 'numbers numbers helicopter helipctoter',
        },
        {
          title: 'Text Editor',
          appName: 'text',
          isOpen: false,
          isHidden: false,
          keepPos: false,
          inFocus: true,
          transition: 'openClose',
          svgPath: 'M10 19.11L12.11 17H7V15H14V15.12L16.12 13H7V11H17V12.12L18.24 10.89C18.72 10.41 19.35 10.14 20.04 10.14C20.37 10.14 20.7 10.21 21 10.33V5C21 3.89 20.1 3 19 3H5C3.89 3 3 3.89 3 5V19C3 20.11 3.9 21 5 21H10V19.11M7 7H17V9H7V7M21.7 14.35L20.7 15.35L18.65 13.3L19.65 12.3C19.86 12.09 20.21 12.09 20.42 12.3L21.7 13.58C21.91 13.79 21.91 14.14 21.7 14.35M12 19.94L18.06 13.88L20.11 15.93L14.06 22H12V19.94Z',
          content: 'text text text text text text',
        },
        {
          title: 'Settings',
          appName: 'settings',
          isOpen: false,
          isHidden: false,
          keepPos: false,
          inFocus: true,
          transition: 'openClose',
          svgPath: 'M12,15.5A3.5,3.5 0 0,1 8.5,12A3.5,3.5 0 0,1 12,8.5A3.5,3.5 0 0,1 15.5,12A3.5,3.5 0 0,1 12,15.5M19.43,12.97C19.47,12.65 19.5,12.33 19.5,12C19.5,11.67 19.47,11.34 19.43,11L21.54,9.37C21.73,9.22 21.78,8.95 21.66,8.73L19.66,5.27C19.54,5.05 19.27,4.96 19.05,5.05L16.56,6.05C16.04,5.66 15.5,5.32 14.87,5.07L14.5,2.42C14.46,2.18 14.25,2 14,2H10C9.75,2 9.54,2.18 9.5,2.42L9.13,5.07C8.5,5.32 7.96,5.66 7.44,6.05L4.95,5.05C4.73,4.96 4.46,5.05 4.34,5.27L2.34,8.73C2.21,8.95 2.27,9.22 2.46,9.37L4.57,11C4.53,11.34 4.5,11.67 4.5,12C4.5,12.33 4.53,12.65 4.57,12.97L2.46,14.63C2.27,14.78 2.21,15.05 2.34,15.27L4.34,18.73C4.46,18.95 4.73,19.03 4.95,18.95L7.44,17.94C7.96,18.34 8.5,18.68 9.13,18.93L9.5,21.58C9.54,21.82 9.75,22 10,22H14C14.25,22 14.46,21.82 14.5,21.58L14.87,18.93C15.5,18.67 16.04,18.34 16.56,17.94L19.05,18.95C19.27,19.03 19.54,18.95 19.66,18.73L21.66,15.27C21.78,15.05 21.73,14.78 21.54,14.63L19.43,12.97Z',
          content: 'welcome to settings',
        },
      ],
    }
  },
  components: {
    AppTaskbar,
    AppDesktop,
    AppContextMenu
  },
  methods: {
    nothing: function (e) {
      e.preventDefault();
    },
    handler: function (e) {
      this.contextVisible = true;
      // document.getElementById('contextmenu').style.opacity = 1;
      if (this.cursorY >= 710) {
        this.cursorY = 710;
      }
      if (this.cursorX >= 1750) {
        this.cursorX = 1750;
      }
      // console.log(`X${this.cursorX}, Y${this.cursorY}`);
      document.getElementById('contextmenu').style.left = this.cursorX + "px";
      document.getElementById('contextmenu').style.top = this.cursorY + "px";
      // alert(`cursor pos is a ${typeof this.cursorX}`)
      e.preventDefault();
      // alert(`yer mouse coordinates are X${this.cursorX} and Y${this.cursorY}`);
    },
    hideContext() {
      if (this.contextVisible === true) {
        this.contextVisible = false;
        // document.getElementById('contextmenu').style.opacity = 0;
        // setTimeout(function () {
        //   document.getElementById('contextmenu').style.left = "-300px";
        //   document.getElementById('contextmenu').style.top = "0px";
        // }, 200)
      }
    },
    mouseMoved(event) {
      this.cursorX = event.clientX;
      this.cursorY = event.clientY;
      document.getElementById('contextmenu').style.left = this.cursorX;
      document.getElementById('contextmenu').style.top = this.cursorY;
      // alert(`X: ${this.cursorX} and Y: ${this.cursorY}`)
    },
    focus(id) {
      switch (id) {
        case 1:
          this.taskbarApps[1]['inFocus'] = true;
          this.taskbarApps[0]['inFocus'] = false;
          this.taskbarApps[2]['inFocus'] = false;
          break;
        case 0:
          this.taskbarApps[1]['inFocus'] = false;
          this.taskbarApps[0]['inFocus'] = true;
          this.taskbarApps[2]['inFocus'] = false;
          break;
        case 2:
          this.taskbarApps[1]['inFocus'] = false;
          this.taskbarApps[0]['inFocus'] = false;
          this.taskbarApps[2]['inFocus'] = true;
          break;

        default:
          alert('wrong input');
          break;
      }
    },
    open(id) {
      var that = this;
      setTimeout(function () {
        that.taskbarApps[id]['isHidden'] = true;
        that.taskbarApps[id]['keepPos'] = false;
      }, 0)
      this.taskbarApps[id]['isHidden'] = false;
      this.taskbarApps[id]['transition'] = 'openClose';
      setTimeout(function () {
        switch (id) {
          case 0:
            that.$modal.show('numbers');
            break;
          case 1:
            that.$modal.show('text');
            break;
          case 2:
            that.$modal.show('settings');
            break;
          default:
            alert('unknown value');
            break;
        }
      }, 0);
    },

    hide(id) {
      var that = this;
      setTimeout(function () {
        that.taskbarApps[id]['isHidden'] = true;
        that.taskbarApps[id]['keepPos'] = true;
      }, 0)
      this.taskbarApps[id]['isOpen'] = true;
      this.taskbarApps[id]['transition'] = 'showHide';
      setTimeout(function () {
        switch (id) {
          case 0:
            that.$modal.hide('numbers');
            break;
          case 1:
            that.$modal.hide('text');
            break;
          case 2:
            that.$modal.hide('settings');
            break;
          default:
            alert('unknown value');
            break;
        }
      }, 0);
    },


    show(id) {
      var that = this;
      setTimeout(function () {
        that.taskbarApps[id]['isHidden'] = false;
        that.taskbarApps[id]['keepPos'] = true;
      }, 0)
      this.taskbarApps[id]['isOpen'] = true;
      this.taskbarApps[id]['transition'] = 'showHide';
      setTimeout(function () {
        switch (id) {
          case 0:
            that.$modal.show('numbers');
            break;
          case 1:
            that.$modal.show('text');
            break;
          case 2:
            that.$modal.show('settings');
            break;
          default:
            alert('unknown value');
            break;
        }
      }, 0);
    },
    close(id) {
      var that = this;
      setTimeout(function () {
        that.taskbarApps[id]['isHidden'] = false;
        that.taskbarApps[id]['keepPos'] = false;
      }, 0)
      this.taskbarApps[id]['transition'] = 'openClose';
      setTimeout(function () {
        switch (id) {
          case 0:
            that.$modal.hide('numbers');
            break;
          case 1:
            that.$modal.hide('text');
            break;
          case 2:
            that.$modal.hide('settings');
            break;
          default:
            alert('unknown value');
            break;
        }
      }, 0);
    }
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  font-family: 'Inter Tight', sans-serif;
  // box-sizing: border-box;
}

.context-enter-active {
  transition: height ease-out .2s, opacity .1s;
}


.context-enter {
  opacity: 0;
  height: 10px !important;
}

.context-leave-active {
  transition: height ease-out .3s, opacity .2s, transform .4s;
}

.context-leave-to {
  opacity: 0;
  // height: 10px !important;
  transform: translateY(100px);
}



.openClose-enter-active,
.openClose-leave-active {
  transition: opacity .2s, transform .2s;
}

.openClose-enter,
.openClose-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

.showHide-enter-active,
.showHide-leave-active {
  transition: opacity .2s, transform .2s;
}

.showHide-enter,
.showHide-leave-to {
  opacity: 0;
  transform: scale(0.9) translate3d(-100px, 100px, 0);
}

.vm--overlay {
  display: none;
}

html body .vm--container {
  pointer-events: none;
  z-index: 2;
}

@import url('https://fonts.googleapis.com/css2?family=Inter+Tight:wght@300;400;500;600&display=swap');

// .vm--overlay {
//   display: none;
// }

#app {
  // font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

html {
  background-image: url(./assets/wallpaper2.jpg);
  background-size: cover;
  overflow-y: hidden;
}

html body div.vm--container.focused {
  z-index: 5;
}

header {
  position: relative;
  top: 0;
  left: 0;
  right: 0;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #52959F;

  span {
    font-weight: 600;
    font-size: 1.2em;
    user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    background-color: #B2C5CB;
    height: 40px;
    display: flex;
    align-items: center;
    text-align: center;
    padding: 0 10px 0 10px;
    color: #000;
    border-radius: 15px 0 15px 0;
  }
}

.content {
  height: 100%;
  width: 100%;
  background-color: #52959F;
}

.window-control {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  // gap: 15px;
  // padding: 0 5px 0 5px;
  // position: absolute;
  // right: 0;
  height: 40px;

  svg {
    width: 20px;
    height: 20px;
  }
}

.hide,
.expand,
.close {
  // border-radius: 15px;
  height: 100%;
  width: 20px;
  background-color: #B2C5CB;
  padding: 0 10px 0 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}



.hide,
.expand {
  border-radius: 0 0 0 15px;

  &:hover {
    background-color: gray;

    svg {
      fill: #fff;
    }
  }

  &:active {
    filter: brightness(0.8);
  }
}

.expand {
  border-radius: 0;
}

.hide svg {
  transform: translateY(5px);
}

.close {
  border-radius: 0 15px 0 0;

  &:hover {
    background-color: rgb(255, 73, 73);

    svg {
      fill: #fff;
    }
  }

  &:active {
    filter: brightness(0.8);
  }
}
</style>
