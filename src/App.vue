<template>
  <div id="app" @mousemove="mouseMoved($event)">
    <AppContextMenu />
    <AppDesktop @contextmenu.native="handler($event)" />
    <modal :class="{ focused: app.inFocus}" :transition="app.transition" :id="'modal-'+index"
      styles="pointer-events: auto; border-radius: 15px;" v-for="(app, index) in taskbarApps" :key="index"
      :name="app.appName" :focusTrap="false" :minWidth="400" :minHeight="200" draggable=".window-header"
      :resizable="true" :adaptive="true" :clickToClose="false" :reset="!app.isHidden">
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
    <AppTaskbar :apps="taskbarApps" @open="open($event)" @hide="hide($event)" @show="show($event)" />

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
      taskbarApps: [
        {
          title: 'Calculator',
          appName: 'numbers',
          isOpen: false,
          isHidden: false,
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
          inFocus: true,
          transition: 'openClose',
          svgPath: 'M10 19.11L12.11 17H7V15H14V15.12L16.12 13H7V11H17V12.12L18.24 10.89C18.72 10.41 19.35 10.14 20.04 10.14C20.37 10.14 20.7 10.21 21 10.33V5C21 3.89 20.1 3 19 3H5C3.89 3 3 3.89 3 5V19C3 20.11 3.9 21 5 21H10V19.11M7 7H17V9H7V7M21.7 14.35L20.7 15.35L18.65 13.3L19.65 12.3C19.86 12.09 20.21 12.09 20.42 12.3L21.7 13.58C21.91 13.79 21.91 14.14 21.7 14.35M12 19.94L18.06 13.88L20.11 15.93L14.06 22H12V19.94Z',
          content: 'text text text text text text',
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
    handler: function (e) {
      document.getElementById('contextmenu').style.opacity = 1;
      e.preventDefault();
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
          break;
        case 0:
          this.taskbarApps[1]['inFocus'] = false;
          this.taskbarApps[0]['inFocus'] = true;
          break;

        default:
          alert('wrong input');
          break;
      }
    },
    open(id) {
      this.taskbarApps[id]['isOpen'] = true;
      this.taskbarApps[id]['isHidden'] = false;
      this.taskbarApps[id]['transition'] = 'openClose';
      var that = this;
      setTimeout(function () {
        switch (id) {
          case 0:
            that.$modal.show('numbers');
            break;
          case 1:
            that.$modal.show('text');
            break;
          default:
            alert('unknown value');
            break;
        }
      }, 0);
    },

    hide(id) {
      this.taskbarApps[id]['isHidden'] = true;
      this.taskbarApps[id]['isOpen'] = true;
      this.taskbarApps[id]['transition'] = 'showHide';
      var that = this;
      setTimeout(function () {
        switch (id) {
          case 0:
            that.$modal.hide('numbers');
            break;
          case 1:
            that.$modal.hide('text');
            break;
          default:
            alert('unknown value');
            break;
        }
      }, 0);
    },


    show(id) {
      this.taskbarApps[id]['isHidden'] = false;
      this.taskbarApps[id]['isOpen'] = true;
      this.taskbarApps[id]['transition'] = 'showHide';
      var that = this;
      setTimeout(function () {
        switch (id) {
          case 0:
            that.$modal.show('numbers');
            break;
          case 1:
            that.$modal.show('text');
            break;
          default:
            alert('unknown value');
            break;
        }
      }, 0);
    },
    close(id) {
      this.taskbarApps[id]['isHidden'] = false;
      this.taskbarApps[id]['isOpen'] = false;
      this.taskbarApps[id]['transition'] = 'openClose';
      var that = this;
      setTimeout(function () {
        switch (id) {
          case 0:
            that.$modal.hide('numbers');
            break;
          case 1:
            that.$modal.hide('text');
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
  background-image: url(./assets/wallpaper.png);
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
