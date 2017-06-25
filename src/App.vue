<template>
  <div id="app">
    <h1>{{msg}}</h1>
    <div class="resize-container">
      <div class="resize-drag">
        Resize from any edge or corner
      </div>
    </div>
  </div>
</template>

<script>
  import interact from 'interactjs'
  export default {
    name: 'app',
    data() {
      return {
        msg: 'Vue Resize Demo'
      }
    },
    mounted() {
      this.init()
    },
    methods: {
      init() {
        interact('.resize-drag')
          .draggable({
            onmove: window.dragMoveListener
          })
          .resizable({
            preserveAspectRatio: true,
            edges: {
              left: true,
              right: true,
              bottom: true,
              top: true
            }
          })
          .on('resizemove', function(event) {
            var target = event.target,
              x = (parseFloat(target.getAttribute('data-x')) || 0),
              y = (parseFloat(target.getAttribute('data-y')) || 0);
            // update the element's style
            target.style.width = event.rect.width + 'px';
            target.style.height = event.rect.height + 'px';
            // translate when resizing from top or left edges
            x += event.deltaRect.left;
            y += event.deltaRect.top;
            target.style.webkitTransform = target.style.transform =
              'translate(' + x + 'px,' + y + 'px)';
            target.setAttribute('data-x', x);
            target.setAttribute('data-y', y);
            target.textContent = Math.round(event.rect.width) + '×' + Math.round(event.rect.height);
          })
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .resize-drag {
    background-color: #29e;
    color: white;
    font-size: 20px;
    font-family: sans-serif;
    border-radius: 20px;
    padding: 20px;
    margin: 20px 20px;
    width: 220px;
    height: 220px;
    /* This makes things *much* easier */
    box-sizing: border-box;
  }
  .resize-container {
    width: 100%;
    height: 600px;
  }
</style>
