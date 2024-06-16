<!--<template>
  <q-page padding>
    <q-uploader
      label="Upload DICOM Files"
      accept=".dcm"
      @added="processFiles"
      @removed="reset"
    />
    <q-linear-progress v-if="isLoading" indeterminate color="primary" />
    <q-img :src="imageSrc" v-if="imageSrc" />
    <q-banner v-if="error" type="negative">
      <template v-slot:avatar>
        <q-icon name="warning" />
      </template>
      <span>{{ error }}</span>
    </q-banner>
  </q-page>
</template>

<script>
import * as dicomParser from 'dicom-parser';

export default {
  name: 'DicomFileUploader',
  data() {
    return {
      files: [],
      imageSrc: null,
      isLoading: false,
      error: null,
    };
  },
  methods: {
    async processFiles() {
      this.isLoading = true;
      this.error = null;
      this.imageSrc = null;

      try {
        const file = this.files[0]; // Assuming single file upload
        console.log('Files selected:', this.files); // Debug statement
        if (!file) {
          throw new Error('No file selected');
        }

        const arrayBuffer = await this.readFileAsArrayBuffer(file);
        const byteArray = new Uint8Array(arrayBuffer);
        const dataSet = dicomParser.parseDicom(byteArray);
        const pixelDataElement = dataSet.elements.x7fe00010;

        if (!pixelDataElement) {
          throw new Error('No pixel data found in the DICOM file');
        }

        const pixelData = new Uint8Array(
          dataSet.byteArray.buffer,
          pixelDataElement.dataOffset,
          pixelDataElement.length
        );

        // Convert pixel data to an image source
        const blob = new Blob([pixelData.buffer], { type: 'image/jpeg' });
        this.imageSrc = URL.createObjectURL(blob);
      } catch (error) {
        this.error = `Error processing DICOM file: ${error.message}`;
        console.error(error);
      } finally {
        this.isLoading = false;
      }
    },
    readFileAsArrayBuffer(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = () => resolve(reader.result);
        reader.onerror = reject;
        reader.readAsArrayBuffer(file);
      });
    },
    reset() {
      this.files = [];
      this.imageSrc = null;
      this.isLoading = false;
      this.error = null;
    },
  },
};
</script>
-->
<!-- src/components/DicomViewer.vue -->

<!--<template>
  <q-page padding>
    <q-uploader
      accept=".dcm"
      @added="handleFileUpload"
      style="max-width: 400px; margin: 0 auto;"
    />
  </q-page>
</template>

<script>
import cornerstone from 'cornerstone-core';
import dicomParser from 'dicom-parser';
import 'cornerstone-wado-image-loader';


export default {
  methods: {
    handleFileUpload(files) {
      // console.log(files[0]); // Check if this logs correctly
      // Further processing logic for handling DICOM files
      const file = files[0];
      const fileReader = new FileReader();

      fileReader.onload = async () => {
        const arrayBuffer = fileReader.result;
        const byteArray = new Uint8Array(arrayBuffer);
        const dataSet = dicomParser.parseDicom(byteArray);

        cornerstoneWADOImageLoader.external.cornerstone = cornerstone;

        // Load image
        const imageId = `dicomweb:${file.name}`;
        cornerstone.loadAndCacheImage(imageId).then(image => {
          const element = this.$refs.dicomViewport;
          cornerstone.enable(element);
          cornerstone.displayImage(element, image);
        }).catch(error => {
          console.error('Error loading image:', error);
        });
      };

      fileReader.readAsArrayBuffer(file);
    }
  },

  props: {
    files: {
      type: Array,
      required: true
    }
  }
};
</script>
-->
<template>
  <div>
    <input type="file" @change="handleFileUpload" />
    <div ref="dicomViewport" style="width: 512px; height: 512px; background: black;"></div>
  </div>
</template>

<script>
import cornerstone from 'cornerstone-core';
import dicomParser from 'dicom-parser';
import cornerstoneWADOImageLoader from 'cornerstone-wado-image-loader';

cornerstoneWADOImageLoader.external.cornerstone = cornerstone;
cornerstoneWADOImageLoader.external.dicomParser = dicomParser;

export default {
  name: 'DicomView',

  mounted() {
    const element = this.$refs.dicomViewport;
    cornerstone.enable(element);
  },

  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        const fileReader = new FileReader();

        fileReader.onload = () => {
          // const arrayBuffer = e.target.result;
          const imageId = cornerstoneWADOImageLoader.wadouri.fileManager.add(file);
          cornerstone.loadImage(imageId).then((image) => {
            const element = this.$refs.dicomViewport;
            cornerstone.displayImage(element, image);
          }).catch((error) => {
            console.error('Error loading image:', error);
          });
        };

        fileReader.readAsArrayBuffer(file);
      }
    },
  },
};
</script>

<style scoped>
/* Add your custom styles here */
</style>
