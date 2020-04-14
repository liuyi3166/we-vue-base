<template>
  <div class="weui-cell weui-cell-list weui-cell_switch">
    <div class="weui-cell__bd">
        {{label}}
        <div class="weui-cell__explan">
            {{placeholder}}
        </div>
    </div>
    <div class="weui-cell__ft" role="switch">
        <input class="weui-switch" ref="input" type="checkbox" 
          :id="id"
          :name="name"
          :true-value="activeValue"
          :false-value="inactiveValue"
          @change="handleChange"
          :disabled="switchDisabled"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'wv-button',
  props: {
    value: {
        type: [Boolean, String, Number],
        default: false
      },
    label:{
      type: String,
      default: ''
    },
    placeholder:{
      type: String,
      default: ''
    },
    name: {
        type: String,
        default: ''
      },
      id: String,
      activeValue: {
        type: [Boolean, String, Number],
        default: true
      },
      inactiveValue: {
        type: [Boolean, String, Number],
        default: false
      },
      disabled: {
        type: Boolean,
        default: false
      }
  },
  data() {
    return {
      
    };
  },
  computed: {
      checked() {
        return this.value === this.activeValue;
      },
      switchDisabled() {
        return this.disabled || (this.elForm || {}).disabled;
      }
    },
    watch: {
      checked() {
        this.$refs.input.checked = this.checked;
      }
    },
  methods:{
     handleChange(event) {
         this.$emit('input', !this.checked ? this.activeValue : this.inactiveValue);
         this.$emit('change', !this.checked ? this.activeValue : this.inactiveValue);
        this.$nextTick(() => {
          this.$refs.input.checked = this.checked;
        });
      },
      switchValue() {
        !this.switchDisabled && this.handleChange();
      },
  },
  created() {
      if (!~[this.activeValue, this.inactiveValue].indexOf(this.value)) {
        this.$emit('input', this.inactiveValue);
      }
    },
  mounted() {
      this.$refs.input.checked = this.checked;
    }
};
</script>