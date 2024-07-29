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

    link:{
      type : 'text',
      placeholder : 'link',
    },
    headline: {
      type: 'text',
      placeholder : 'headline'
    },

    content: {
      type: 'editor',
    },

    button : {
      type : 'text',
      placeholder : 'button',

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

  <div id="image">
    <img src="../src/assets/1kqp4d.jpg" alt="Girl in a jacket" width="500" height="600">


  </div>
  <div id="widget-content">
    <h1> {{ formValues.headline }}</h1>
    <div v-html="formValues.content"></div>
  </div>
</template>