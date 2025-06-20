<template>
  <DefaultField
    :field="currentField"
    :errors="errors"
    :show-help-text="showHelpText"
    :full-width-content="fullWidthContent"
  >
    <template #field>
        <div class="flex items-stretch w-full mb-4 relative">
            <div class="flex -mr-px w-auto" v-if="hasContentBefore">
                <span 
                  class="w-full flex px-3 bg-40 form-input-bordered rounded-l items-center border-r-0"
                  :class="prependExtraClasses"
                >
                  <display-as :iconPosition="currentField.prependIconPosition" :icon="currentField.prependIcon" :value="currentField.prepend" :html="currentField.asHtml" />
                 </span> 
            </div>
            <input
                :id="currentField.uniqueKey"
                :dusk="field.attribute"
                :name="field.name"
                type="text"
                :class="classObject"
                :placeholder="currentField.placeholder || currentField.name"
                v-model="value"
            />
            <div class="flex -mr-px w-auto" v-if="hasContentAfter">
                <span 
                  class="w-full flex px-3 bg-40 form-input-bordered rounded-r items-center border-l-0"
                  :class="appendExtraClasses"
                >
                  <display-as :iconPosition="currentField.appendIconPosition" :icon="currentField.appendIcon" :value="currentField.append" :html="currentField.asHtml" />
               </span> 
            </div>
            
        </div>
    </template>
  </DefaultField>
</template>

<script>
import { DependentFormField, HandlesValidationErrors } from 'laravel-nova'
import DisplayAs from './DisplayAs.vue'

export default {
  mixins: [DependentFormField, HandlesValidationErrors],
  components: {
    DisplayAs,
  },
  props: ['resourceName', 'resourceId', 'field'],
  computed: {
    prependExtraClasses() {
        let extraClasses = this.currentField.prependExtraClasses || 'bg-gray-100';
        return extraClasses;
    },
    appendExtraClasses() {
      let extraClasses = this.currentField.appendExtraClasses || 'bg-gray-100';
        return extraClasses;
    },
    hasContentBefore() {
      return this.currentField.prependIcon || this.currentField.prepend
    },
    hasContentAfter() {
      return this.currentField.appendIcon || this.currentField.append
    },
    classObject() {
      let inputClass = 'form-control form-input form-input-bordered';

      if (this.hasContentBefore && this.hasContentAfter) {
          inputClass+= ' rounded-none';
          return inputClass;
      }

      if (this.hasContentBefore) {
          inputClass+=' w-3/5 rounded-l-none'
      }
      if (this.hasContentAfter) {
          inputClass+=' w-3/5 rounded-r-none'
      }

      return inputClass;
    },
  },
  methods: {
    /*
     * Set the initial, internal value for the field.
     */
    setInitialValue() {
      this.value = this.currentField.value || ''
    },

    /**
     * Fill the given FormData object with the field's internal value.
     */
    fill(formData) {
      formData.append(this.currentField.attribute, this.value || '')
    },
  },
}
</script>
