<!-- src/pages/IndexPage.vue -->
<template>
  <q-page>
    <div class="q-pa-md">
      <h1>DICOM Viewer</h1>
      <DicomViewer />
    </div>
  </q-page>
</template>

<script>
import DicomViewer from '../components/DicomView.vue';

export default {
  components: {
    DicomViewer,
  },
};
</script>

<style scoped>
/* Add your custom styles */
</style>

<!--<template>
  <q-page padding>
    <DicomUploader />
  </q-page>
</template>

<script>
import DicomUploader from 'components/Dicomview.vue';

export default {
  components: {
    DicomUploader,
  },
};
</script>
-->
<!--<template>
  <q-page class="row items-center justify-evenly">
    <example-component
      title="Example component"
      active
      :todos="todos"
      :meta="meta"
    ></example-component>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { Todo, Meta } from 'components/models';
import ExampleComponent from 'components/ExampleComponent.vue';

defineOptions({
  name: 'IndexPage'
});


</script>
-->
