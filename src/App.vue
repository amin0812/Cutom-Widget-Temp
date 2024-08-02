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
    separation: {
      type: 'static',
      tag: 'hr',
    },
    buttonName: {
      type: 'text',
      placeholder: 'Button Name',
    },
    buttonLink: {
      type: 'text',
      placeholder: 'Button Link',
    },
    separation1: {
      type: 'static',
      tag: 'hr',
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

  <table id="widget-content" style="width: 100%; background-color: #f0f0f0; padding: 10px; border-radius: 5px; border-spacing: 10px;">
    <tbody>
      <!-- Image row -->
      <tr v-if="formValues.link">
        <td style="width: 100px; vertical-align: top;">
          <img :src="formValues.link" alt="Image" style="width: 100%; height: auto; object-fit: cover;">
        </td>
        <td style="vertical-align: top;">
          <!-- Headline, content, and button -->
          <table style="width: 100%;">
            <tbody>
              <tr>
                <td style="text-align: left;">
                  <h3 style="margin: 0 0 10px 0;">{{ formValues.headline }}</h3>
                </td>
              </tr>
              <tr>
                <td style="text-align: left;" v-html="formValues.content"></td>
              </tr>
              <tr v-if="formValues.buttonName">
                <td style="text-align: left;">
                  <a :href="formValues.buttonLink" style="text-decoration: none;">
                    <button style="display: block; width: 100%; padding: 10px; background-color: #007bff; color: #fff; border: none; border-radius: 5px; cursor: pointer;">
                      {{ formValues.buttonName }}
                    </button>
                  </a>
                </td>
              </tr>
            </tbody>
          </table>
        </td>
      </tr>
      <tr v-if="!formValues.link">
        <td colspan="2">
          <!-- Headline, content, and button when no image -->
          <table style="width: 100%;">
            <tbody>
              <tr>
                <td style="text-align: left;">
                  <h3 style="margin: 0 0 10px 0;">{{ formValues.headline }}</h3>
                </td>
              </tr>
              <tr>
                <td style="text-align: left;" v-html="formValues.content"></td>
              </tr>
              <tr v-if="formValues.buttonName">
                <td style="text-align: left;">
                  <a :href="formValues.buttonLink" style="text-decoration: none;">
                    <button style="display: block; width: 100%; padding: 10px; background-color: #007bff; color: #fff; border: none; border-radius: 5px; cursor: pointer;">
                      {{ formValues.buttonName }}
                    </button>
                  </a>
                </td>
              </tr>
            </tbody>
          </table>
        </td>
      </tr>
      <tr v-if="formValues.phone || formValues.email">
        <td colspan="2">
          <!-- Contact Information -->
          <table style="width: 100%; border-spacing: 0;">
            <tbody>
              <tr v-if="formValues.phone">
                <td style="padding: 5px; vertical-align: middle;">
                  <i class="fas fa-phone-alt" style="margin-right: 5px;"></i>
                  {{ formValues.phone }}
                </td>
              </tr>
              <tr v-if="formValues.email">
                <td style="padding: 5px; vertical-align: middle;">
                  <i class="fas fa-envelope" style="margin-right: 5px;"></i>
                  {{ formValues.email }}
                </td>
              </tr>
            </tbody>
          </table>
        </td>
      </tr>
    </tbody>
  </table>
</template>
