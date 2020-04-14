<template>
  <label
    class="weui-cell weui-cell-list weui-check__label"
    role="radio"
    :aria-checked="model === label"
    :aria-disabled="isDisabled"
    :tabindex="tabIndex"
    @keydown.space.stop.prevent="model = isDisabled ? model : label"
  >
    <div class="weui-cell__bd">
      <p>{{text}}</p>
      <div class="weui-cell__explan" v-if="content">
        {{content}}
      </div>
    </div>
    <div class="weui-cell__ft">
      <span v-if="!content">{{placeholder}}</span>
      <span>
        <input
          class="weui-check"
          :value="label"
          type="radio"
          aria-hidden="true"
          v-model="model"
          @focus="focus = true"
          @blur="focus = false"
          @change="handleChange"
          :name="name"
          :disabled="isDisabled"
          tabindex="-1"
        >
        <span class="weui-icon-checked"></span>
      </span>
    </div>
  </label>
</template>
<script>
  import Emitter from '../../mixins/emitter';

  export default {
    name: 'WvRadio',

    mixins: [Emitter],

    inject: {
      elForm: {
        default: ''
      },

      elFormItem: {
        default: ''
      }
    },

    componentName: 'WvRadio',

    props: {
      value: {},
      label: {},
      disabled: Boolean,
      name: String,
      content:String,
      placeholder:String,
      text:String,
      border: Boolean,
      size: String
    },

    data() {
      return {
        focus: false
      };
    },
    computed: {
      isGroup() {
        let parent = this.$parent;
        while (parent) {
          if (parent.$options.componentName !== 'WvRadioGroup') {
            parent = parent.$parent;
          } else {
            return true;
          }
        }
        return false;
      },
      model: {
        get() {
          return this.isGroup ? this._radioGroup.value : this.value;
        },
        set(val) {
          if (this.isGroup) {
            this.dispatch('WvRadioGroup', 'input', [val]);
          } else {
            this.$emit('input', val);
          }
        }
      },
      _elFormItemSize() {
        return (this.elFormItem || {}).elFormItemSize;
      },
      radioSize() {
        const temRadioSize = this.size || this._elFormItemSize || (this.$ELEMENT || {}).size;
        return this.isGroup
          ? this._radioGroup.radioGroupSize || temRadioSize
          : temRadioSize;
      },
      isDisabled() {
        return this.isGroup
          ? this._radioGroup.disabled || this.disabled || (this.elForm || {}).disabled
          : this.disabled || (this.elForm || {}).disabled;
      },
      tabIndex() {
        return !this.isDisabled ? (this.isGroup ? (this.model === this.label ? 0 : -1) : 0) : -1;
      }
    },
    created() {
     if(this.$parent.$options.componentName === 'WvRadioGroup'){
       this._radioGroup = this.$parent
     }
    },
    methods: {
      handleChange() {
        this.$nextTick(() => {
          this.$emit('change', this.model);
          this.isGroup && this.dispatch('WvRadioGroup', 'handleChange', this.model);
        });
      }
    }
  };
</script>
