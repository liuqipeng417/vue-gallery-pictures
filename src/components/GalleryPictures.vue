<template>
	<gallery-layout
    :class="{
      'gallery-pictures-container-origin': isOriginMode
    }"
    :items="items"
    :box-init-ratio="boxInitRatio"
    :box-container-class="boxContainerClass">
      <template slot-scope="scope">
        <picture-box
          :src="scope.item.src"
          :loading.sync="scope.item.loading"
          :error.sync="scope.item.error"
          @error="$emit('error', scope.item, $event)"
          @loaded="$emit('loaded', scope.item)">
        </picture-box>
        <slot :item="scope.item"></slot>
      </template>
  </gallery-layout>
</template>

<script>
  import GalleryLayout from 'vue-gallery-layout';
  import PictureBox from './PictureBox.vue';

	export default {
    name: 'GalleryPictures',

    components: {
      GalleryLayout,
      PictureBox
    },

		props: {
			items: {
				type: Array,
				default() {
          return [];
        }
      },
      boxInitRatio: {
        type: Number,
        default: 200
      },
      boxContainerClass: {
        type: String,
        default: ''
      },
      lastLineMode: {
        type: String,
        default: 'full'
      }
    },

    computed: {
      isOriginMode() {
        return this.lastLineMode === 'origin';
      }
    }
	};
</script>

<style>
  .gallery-pictures-container-origin::after {
    content: '';
    flex-grow: 99999999;
    min-width: 100px;
  }
</style>
