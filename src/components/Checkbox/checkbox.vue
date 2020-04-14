<template>
<div class="weui-cells" :class="[classType]">
  <label
    class="weui-cell weui-cell-list weui-check__label"
    role="checkbox"
    :aria-checked="indeterminate ? 'mixed': isChecked"
    :aria-disabled="isDisabled"
    :id="id"
  >
    <div class="weui-cell__hd">
      <input
        v-if="trueLabel || falseLabel"
        class="weui-check"
        type="checkbox"
        aria-hidden="true"
        :disabled="isDisabled"
        :value="label"
        :name="name"
        v-model="model"
        @change="handleChange"
        @focus="focus = true"
        @blur="focus = false">
       <input
        v-else
        class="weui-check"
        type="checkbox"
        aria-hidden="true"
        :disabled="isDisabled"
        :value="label"
        :name="name"
        v-model="model"
        @change="handleChange"
        @focus="focus = true"
        @blur="focus = false">
        <i :class="[iconClassType]"></i>
    </div>
    <div class="weui-cell__bd">
      <p>{{label}}</p>
      <div class="weui-cell__explan">
          {{placeholder}}
      </div>
     </div>
  </label>
  </div>
</template>
<script>
  import Emitter from '../../mixins/emitter';

  export default {
    name: 'WvCheckbox',

    mixins: [Emitter],

    inject: {
      WvForm: {
        default: ''
      },
      WvFormItem: {
        default: ''
      }
    },

    componentName: 'WvCheckbox',

    data() {
      return {
        selfModel: false,
        focus: false,
        isLimitExceeded: false,
        classType:this.type === 'circular'? 'weui-cells_checkbox' : 'weui-cells_checkbox2',
        iconClassType:this.type === 'circular'? 'weui-icon-checked' : 'weui-checkbox2-checked',
      };
    },

    computed: {
      model: {
        get() {
          return this.isGroup
            ? this.store : this.value !== undefined
              ? this.value : this.selfModel;
        },
        set(val) {
          if (this.isGroup) {
           this.isLimitExceeded = false;
            (this._checkboxGroup.min !== undefined &&
              val.length < this._checkboxGroup.min &&
              (this.isLimitExceeded = true));
            (this._checkboxGroup.max !== undefined &&
              val.length > this._checkboxGroup.max &&
              (this.isLimitExceeded = true));

            this.isLimitExceeded === false &&
            this.dispatch('WvCheckboxGroup', 'input', [val]);
          } else {
            this.$emit('input', val);
            this.selfModel = val;
          }
        }
      },
      isChecked() {
        if ({}.toString.call(this.model) === '[object Boolean]') {
          return this.model;
        } else if (Array.isArray(this.model)) {
          return this.model.indexOf(this.label) > -1;
        } else if (this.model !== null && this.model !== undefined) {
          return this.model === this.trueLabel;
        } else {
         return false
        }
      },
      isGroup() {
        let parent = this.$parent;
        while (parent) {
          if (parent.$options.componentName !== 'WvCheckboxGroup') {
            parent = parent.$parent;
          } else {
            return true;
          }
        }
        return false;
      },
      store() {
        return this._checkboxGroup ? this._checkboxGroup.value : this.value;
      },

      isDisabled() {
        return this.isGroup
          ? this._checkboxGroup.disabled || this.disabled || (this.WvForm || {}).disabled
          : this.disabled || (this.WvForm || {}).disabled;
      },

      _WvFormItemSize() {
        return (this.WvFormItem || {}).WvFormItemSize;
      },

      checkboxSize() {
        const temCheckboxSize = this.size || this._WvFormItemSize || (this.$ELEMENT || {}).size;
        return this.isGroup
          ? this._checkboxGroup.checkboxGroupSize || temCheckboxSize
          : temCheckboxSize;
      }
    },

    props: {
      type:{
        type: String,
        default: 'square'  // square：方形  circular：圆形
      },
      value: {},
      label: {},
      placeholder: String,
      indeterminate: Boolean,
      disabled: Boolean,
      checked: Boolean,
      name: String,
      trueLabel: [String, Number],
      falseLabel: [String, Number],
      id: String, /* 当indeterminate为真时，为controls提供相关连的checkbox的id，表明元素间的控制关系*/
      controls: String, /* 当indeterminate为真时，为controls提供相关连的checkbox的id，表明元素间的控制关系*/
      border: Boolean,
      size: String
    },

    methods: {
      addToStore() {
        if (
          Array.isArray(this.model) &&
          this.model.indexOf(this.label) === -1
        ) {
          this.model.push(this.label);
        } else {
          this.model = this.trueLabel || true;
        }
      },
      handleChange(ev) {
        if (this.isLimitExceeded) return;
        let value;
        if (ev.target.checked) {
          value = this.trueLabel === undefined ? true : this.trueLabel;
        } else {
          value = this.falseLabel === undefined ? false : this.falseLabel;
        }
        this.$emit('change', value, ev);
        this.$nextTick(() => {
          if (this.isGroup) {
            this.dispatch('WvCheckboxGroup', 'change', [this._checkboxGroup.value]);
          }
        });
      }
    },

    created() {
     if(this.$parent.$options.componentName === 'WvCheckboxGroup'){
       this._checkboxGroup = this.$parent
     }
      this.checked && this.addToStore();
    }
  };
</script>
