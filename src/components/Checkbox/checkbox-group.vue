<script>
  import Emitter from '../../mixins/emitter';
  export default {
    name: 'WvCheckboxGroup',

    componentName: 'WvCheckboxGroup',

    mixins: [Emitter],

    inject: {
      WvFormItem: {
        default: ''
      }
    },

    props: {
      value: {},
      disabled: Boolean,
      min: Number,
      max: Number,
      size: String,
      fill: String,
      textColor: String
    },

    computed: {
      _WvFormItemSize() {
        return (this.WvFormItem || {}).WvFormItemSize;
      },
      checkboxGroupSize() {
        return this.size || this._WvFormItemSize || (this.$ELEMENT || {}).size;
      }
    },
    
    watch: {
      value(value) {
        console.log('dddddddddddddddd===', value)
      }
    }
  };
</script>

<template>
  <div class="weui-cells weui-cells_checkbox" role="group" aria-label="checkbox-group">
    <slot></slot>
  </div>
</template>
