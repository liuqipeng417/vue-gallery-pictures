<template>
  <div class="gallery-pic-box-container">
    <img
      v-show="!loading && !error"
      :src="actualSrc"
      class="gallery-pic-box-img"/>
  </div>
</template>

<script>
  export default {
    name: 'PictureBox',

    props: {
      src: {
        type: String,
        default: ''
      },
      loading: {
        type: Boolean,
        default: true
      },
      error: {
        type: Boolean,
        default: true
      }
    },

    data() {
      return {
        actualSrc: ''
      };
    },

    watch: {
      src: {
        immediate: true,
        handler() {
          this.loadImage();
        }
      }
    },

    methods: {
      loadImage() {
        this.loading = true;
        this.actualSrc = '';
        this.imageLoader(this.src).then((res) => {
          this.$emit('update:loading', false);
          this.$emit('loaded');
          this.actualSrc = this.src;
        }).catch((e) => {
          this.$emit('update:error', true);
          this.$emit('error', e);
        });
      },

      imageLoader(src) {
        return new Promise((resolve, reject) => {
          let image = new Image();
          image.onload = () => {
            resolve();
          }

          image.onerror = (e) => {
            reject(new Error(`Load Picture Error - ${src}`));
          }

          image.src = src;
        });
      }
    }
  }
</script>

<style>
  .gallery-pic-box-container {
    height: 100%;
  }

  .gallery-pic-box-img {
    width: 100%;
  }
</style>

