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
      type:{
        type: String,
        default: 'square'  // square：方形  circular：圆形
      },
      disabled: Boolean,
      min: Number,
      max: Number,
      size: String,
      fill: String,
      textColor: String
    },
    data() {
      return {
        classType:this.type === 'circular'? 'weui-cells_checkbox' : 'weui-cells_checkbox2'
      };
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
  <div class="weui-cells" :class="[classType]" role="group" aria-label="checkbox-group">
    <slot></slot>
  </div>
</template>
