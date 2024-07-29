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
      <img :src="formValues.link || '../src/assets/1kqp4d.jpg'" alt="Girl in a jacket">
    </div>
    <div id="text-content">
      <h3>{{ formValues.headline }}</h3>
      <div v-html="formValues.content"></div>
      <div class="button">
        <button type="button">{{formValues.button}}</button>
      </div>
    </div>

  </div>
</template>

<style scoped>
.banner {
  display: flex;
  text-align: left;
  align-items: flex-start;
  background-color: rgb(241, 236, 229);
  /* Align items to the start of the flex container */
}

.image {
  flex-shrink: 0;
  /* Prevent the image from shrinking */
  margin-right: 20px;
  /* Space between the image and the text content */
}

.image img {
  width: 215;
  height: 100px;
  object-fit: cover;
}

#text-content {
  flex-grow: 1;
  /* Allow the text content to grow and take up the remaining space */
  display: flex;
  flex-direction: column;
  /* Stack the headline and content vertically */
}

h3 {
  margin: 0;
  /* Remove default margin from the headline */
}
</style>
