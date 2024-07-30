<script setup>
import { onMounted, onUpdated, ref, unref } from 'vue';
import { cloneDeep } from 'lodash';
import sdkclass from 'blocksdk';

// If using npm/yarn, import Font Awesome CSS
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

  <table id="widget-content" style="width: 100%; background-color: #f0f0f0; padding: 10px; border-radius: 5px; border-spacing: 0; border-collapse: collapse;">
    <tr>
      <template v-if="formValues.link">
        <td style="width: 100px; vertical-align: top; padding-right: 10px;">
          <img :src="formValues.link" alt="Image" style="width: 100px; height: 100px; object-fit: cover;">
        </td>
      </template>
      <td style="vertical-align: top; text-align: left; padding-left: 10px;" :colspan="formValues.link ? 1 : 2">
        <h3 style="margin: 0 0 10px 0;">{{ formValues.headline }}</h3>
        <div style="margin-bottom: 10px;" v-html="formValues.content"></div>
        <button v-if="formValues.button" style="padding: 8px 16px; font-size: 14px; cursor: pointer;">{{ formValues.button }}</button>
        <template v-if="formValues.phone">
          <table style="width: 100%; margin-top: 10px; border-collapse: collapse;">
            <tr>
              <td style="width: 20px; padding-right: 5px;">
                <i class="fas fa-phone-alt" style="font-size: 16px;"></i>
              </td>
              <td>
                {{ formValues.phone }}
              </td>
            </tr>
          </table>
        </template>
        <template v-if="formValues.email">
          <table style="width: 100%; margin-top: 10px; border-collapse: collapse;">
            <tr>
              <td style="width: 20px; padding-right: 5px;">
                <i class="fas fa-envelope" style="font-size: 16px;"></i>
              </td>
              <td>
                {{ formValues.email }}
              </td>
            </tr>
          </table>
        </template>
      </td>
    </tr>
  </table>
</template>
