<script setup>
import { onMounted, onUpdated, ref, unref } from 'vue';
import { cloneDeep } from 'lodash';
import sdkclass from 'blocksdk';

// Import Font Awesome CSS for icons
import '@fortawesome/fontawesome-free/css/all.min.css';

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
    separation : {
      type  : 'static',
      tag: 'hr',
    },
    button: {
      type: 'text',
      placeholder: 'Button Link',
    },
    phone: {
      type: 'phone',
      placeholder: 'Phone'
    },
    email: {
      type: 'text',
      inputType: 'email',
      rules: [
        'required',
        'max:255',
        'email',
      ],
      placeholder: 'Email',
    },
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

  <table width="100%" bgcolor="#f0f0f0" cellpadding="10" cellspacing="0" style="border-radius: 5px; border-spacing: 0;">
    <tr>
      <template v-if="formValues.link">
        <td width="100" valign="top" style="padding: 0;">
          <img :src="formValues.link" alt="Image" width="100" height="100" style="display: block; width: 100px; height: 100px; object-fit: cover;">
        </td>
      </template>
      <td valign="top">
        <table width="100%" cellpadding="0" cellspacing="0">
          <tr>
            <td style="padding-bottom: 10px;">
              <h3 style="margin: 0;">{{ formValues.headline }}</h3>
            </td>
          </tr>
          <tr>
            <td style="padding-bottom: 10px;">
              <span v-html="formValues.content"></span>
            </td>
          </tr>
          <tr v-if="formValues.button">
            <td style="padding-bottom: 10px;">
              <button style="padding: 8px 16px; font-size: 14px; cursor: pointer;">{{ formValues.button }}</button>
            </td>
          </tr>
          <tr v-if="formValues.phone">
            <td style="padding-bottom: 10px; display: flex; align-items: center;">
              <i class="fas fa-phone-alt" style="margin-right: 5px;"></i>
              {{ formValues.phone }}
            </td>
          </tr>
          <tr v-if="formValues.email">
            <td style="padding-bottom: 10px; display: flex; align-items: center;">
              <i class="fas fa-envelope" style="margin-right: 5px;"></i>
              {{ formValues.email }}
            </td>
          </tr>
        </table>
      </td>
    </tr>
  </table>
</template>
