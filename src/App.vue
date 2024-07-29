<script setup>
import { onMounted, onUpdated, ref, unref } from 'vue';
import { cloneDeep } from 'lodash';
import sdkclass from 'blocksdk';

const sdk = new sdkclass();

const formValues = ref({});

const vueform = ref({
  size: 'md',
  displayErrors: false,
  onChange: (Values) => { formValues.value = Values },
  schema: {
    title: {
      type: 'static',
      content: 'Custom Widget',
      tag: 'h1'
    },
    link: {
      type: 'text',
      placeholder: 'Image URL',
    },
    headline: {
      type: 'text',
      placeholder: 'headline'
    },
    content: {
      type: 'editor',
    },
    button: {
      type: 'text',
      placeholder: 'button',
    }
  }
});

onMounted(() => {
  sdk.getData((data) => {
    formValues.value = data;
    vueform.value.default = data;
    updateKey.value++;
  });
});

onUpdated(() => {
  const content = document.querySelector("#widget-content").innerHTML;
  sdk.setData(cloneDeep(unref(formValues)));
  sdk.setContent(content);
});

const updateKey = ref(0);
</script>

<template>
  <Vueform :key="updateKey" v-bind="vueform" />

  <table id="widget-content" class="banner">
    <tr>
      <td class="image-cell">
        <img :src="formValues.link || '../src/assets/1kqp4d.jpg'" alt="Image">
      </td>
      <td class="text-content">
        <h3>{{ formValues.headline }}</h3>
        <div v-html="formValues.content"></div>
        <button>{{formValues.button }}</button>
      </td>
    </tr>
  </table>
</template>

<style scoped>
.banner {
  width: 100%;
  background-color: #f0f0f0; /* Light grey background color */
  padding: 10px; /* Add some padding for better appearance */
  border-radius: 5px; /* Optional: Add border radius for rounded corners */
  border-spacing: 10px; /* Add space between table cells */
}

.image-cell {
  width: 100px;
  vertical-align: top;
}

.image-cell img {
  width: 100px;
  height: 100px;
  object-fit: cover; /* Ensure the image fits within the specified dimensions */
}

.text-content {
  vertical-align: top;
  text-align: left;
}

h3 {
  margin: 0 0 10px 0;
}
</style>
