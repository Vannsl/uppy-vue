<template>
  <div :id="uppyId">
    <PhotoGrid :files="files"/>
    <div class="ThumbnailContainer" v-if="collection === 'thumbnail'">
      <button id="open-thumbnail-modal" class="button">Select file</button>

    </div>
    <div class="DashboardContainer" v-else></div>
  </div>
</template>

<script>
import PhotoGrid from './PhotoGrid';

const Uppy = require('uppy/lib/core');
const Dashboard = require('uppy/lib/plugins/Dashboard');
const XHRUpload = require('uppy/lib/plugins/XHRUpload');

export default {
  props: {
    modelId: {
      type: String,
      required: true,
    },
    collection: {
      type: String,
      required: true,
    },
    endpoint: {
      type: String,
      required: false,
      default() {
        return '/upload';
      },
    },
  },

  components: {
    PhotoBox,
  },

  data() {
    return {
      files: {},
    };
  },

  computed: {
    uppyId() {
      return `${this.modelId}-${this.collection}`;
    },
  },

  mounted() {
    const uppy = Uppy({
      id: this.uppyId,
      autoProceed: false,
      debug: true,
      thumbnailGeneration: true,
      restrictions: {
        maxFileSize: false,
        allowedFileTypes: ['image/*', 'application/pdf'],
      },
      meta: {
        modelId: this.modelId,
        collection: this.collection,
      },
      onBeforeFileAdded: () => {
        Promise.resolve();
      },
      onBeforeUpload: (files) => {
        this.files = files;
        Promise.resolve();
      },
    });
    if (this.collection === 'thumbnail') {
      uppy.use(Dashboard, {
        trigger: '#open-thumbnail-modal',
        metaFields: [
          { id: 'owner', name: 'Owner', placeholder: 'name of the photographer/owner' },
          { id: 'caption', name: 'Caption', placeholder: 'describe what the image is about' },
          { id: 'order', name: 'Order', placeholder: 'order' },
        ],
      });
    } else {
      uppy.use(Dashboard, {
        inline: true,
        target: '.DashboardContainer',
        replaceTargetContent: true,
        note: 'Images and PDF only.',
        maxHeight: 500,
        metaFields: [
          { id: 'owner', name: 'Owner', placeholder: 'name of the photographer/owner' },
          { id: 'caption', name: 'Caption', placeholder: 'describe what the image is about' },
          { id: 'order', name: 'Order', placeholder: 'order' },
        ],
      });
    }
    uppy.use(XHRUpload, {
      endpoint: this.endpoint,
      headers: {
        'X-CSRF-TOKEN': window.csrfToken,
      },
    });
    // uppy.on('upload-success', (fileId, resp, uploadURL) => {
    //   debugger;
    //   console.log(uploadURL)
    //   var img = new Image()
    //   img.width = 300
    //   img.alt = fileId
    //   img.src = uploadURL
    //   document.body.appendChild(img)
    // });
    uppy.run();
  },

  methods: {},
}; 
</script>

<style src="uppy/dist/uppy.css"></style>
