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
      placeholder: 'link',
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

})

onMounted(() => {
  sdk.getData((data) => {
    formValues.value = data;
    vueform.value.default = data;
    updateKey.value++;
  })
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


  <div id="widget-content" class="banner">
    <div class="image">
      <img src="../src/assets/1kqp4d.jpg" alt="Girl in a jacket">

    </div>
    <h1> {{ formValues.headline }}</h1>
    <div v-html="formValues.content"></div>
  </div>
</template>

<style scoped>
.banner {
  display: flex;
  align-items: flex-start; /* Align items to the start of the flex container */
}

.image {
  flex-shrink: 0; /* Prevent the image from shrinking */
  margin-right: 20px; /* Space between the image and the text content */
}

.image img {
  width: 100px;
  height: 100px;
}

#text-content {
  flex-grow: 1; /* Allow the text content to grow and take up the remaining space */
}
</style>
