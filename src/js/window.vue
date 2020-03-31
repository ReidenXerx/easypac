<template>
  <div class="sexy-window" v-bind:class="{ 'w-min': minimized, 'w-max': maximized }" v-bind:style="{left: this.coords.left + 'px', top: this.coords.top + 'px', transition: '0s'}" v-on:mousemove="elementDrag">
    <div class="sexy-window-head flex a-i-center j-c-s-between" v-on:mousedown="mouseDown" v-on:mouseup="mouseUp">
      <p>{{header}}</p>
      <div>
        <i class="fas fa-window-minimize" v-on:click="minimizeWindow" v-if="ismin"></i>
        <i class="far fa-window-maximize" v-on:click="maximizeWindow" v-if="ismax"></i>
        <i class="fas fa-times-circle" v-if="isdelete"></i>
      </div>
    </div>
    <div class="sexy-window-body">
      <slot name="window-body">default window body</slot>
    </div>
  </div>
</template>

<script>
export default {
  name: 'sexywindow',
  props: {
    header: '',
    ismin: true,
    ismax: true,
    isdelete: true
  },
  mounted() {
    //this.coords.x = document.getElementById('sw').getBoundingClientRect().left,
    //this.coords.y = document.getElementById('sw').getBoundingClientRect().top
  },
  data() {
    return {
      coords: {
        left: 300,
        top: 300,
        last_left: 300,
        last_top: 300
      },

      p: {
        pos1: 0,
        pos2: 0,
        pos3: 0,
        pos4: 0
      },
      minimized: false,
      maximized: false,
      draggable: true,
      mousedown: false
    }
  },
  methods: {
    closeWindow: function() {
      //// TODO: anus
    },
    minimizeWindow: function(e) {
      this.minimized=!this.minimized;
      this.draggable=!this.draggable;
    },
    maximizeWindow: function(e) {
      if(!this.maximized) {
        this.coords.left = 0;
        this.coords.top = 0;
      }
      else {
        this.coords.left = this.coords.last_left;
        this.coords.top = this.coords.last_top;
      }
      this.maximized=!this.maximized;
      this.draggable=!this.draggable;
    },
    mouseDown: function(e) {
      this.mousedown = true;
      this.p.pos3 = e.clientX;
      this.p.pos4 = e.clientY;
    },
    mouseUp: function(e) {
      this.mousedown = false;
    },
    elementDrag: function(e) {
      if(this.mousedown && this.draggable) {
        e = e || window.event;
        e.preventDefault();
        // calculate the new cursor position:
        this.p.pos1 = this.p.pos3 - e.clientX;
        this.p.pos2 = this.p.pos4 - e.clientY;
        this.p.pos3 = e.clientX;
        this.p.pos4 = e.clientY;
        // set the element's new position:
        this.coords.top = (this.coords.top - this.p.pos2);
        this.coords.left = (this.coords.left - this.p.pos1);
        this.coords.last_top = this.coords.top;
        this.coords.last_left = this.coords.left;
      }
    }
  },
  send() {

  }
}


</script>
