<template>
<div class="weui-slider-box">
  <div class="weui-slider"
     role="slider"
     :aria-valuemin="min"
     :aria-valuemax="max"
  >
    <div class="weui-slider__inner"
      @click="onSliderClick"
      ref="slider">
      <div
        class="weui-slider__track"
        :style="barStyle">
      </div>
      <div
        class="weui-slider__handler"
        :style="handlerStyle">
      </div>
    </div>
  </div>
  <div id="sliderValue" class="weui-slider-box__value" v-if="showBar">{{firstValue}}</div>
  </div>
</template>

<script type="text/babel">
  import Emitter from '../../mixins/emitter';

  export default {
    name: 'WvSlider',

    mixins: [Emitter],

    inject: {
      elForm: {
        default: ''
      }
    },

    props: {
      min: {
        type: Number,
        default: 0
      },
      max: {
        type: Number,
        default: 100
      },
      value: {
        type: [Number, Array],
        default: 0
      },
      showBar: {
        type: Boolean,
        default: false
      },
      label: {
        type: String
      }
    },

    data() {
      return {
        firstValue: null,
        oldValue: null,
        dragging: false,
        sliderSize: 1
      };
    },

    watch: {
      value(val, oldVal) {
        if (this.dragging ||
          Array.isArray(val) &&
          Array.isArray(oldVal) &&
          val.every((item, index) => item === oldVal[index])) {
          return;
        }
        this.setValues();
      },

      dragging(val) {
        if (!val) {
          this.setValues();
        }
      },

      firstValue(val) {
       this.$emit('input', val);
      },

      min() {
        this.setValues();
      },

      max() {
        this.setValues();
      }
    },

    methods: {
      valueChanged() {
        return this.value !== this.oldValue;
      },
      setValues() {
        if (this.min > this.max) {
          console.error('[Element Error][Slider]min should not be greater than max.');
          return;
        }
        const val = this.value;
        if (typeof val === 'number' && !isNaN(val)) {
          if (val < this.min) {
            this.$emit('input', this.min);
          } else if (val > this.max) {
            this.$emit('input', this.max);
          } else {
            this.firstValue = val;
            if (this.valueChanged()) {
              this.dispatch('ElFormItem', 'el.form.change', val);
              this.oldValue = val;
            }
          }
        }
      },

      setPosition(percent) {
        const targetValue = this.min + percent * (this.max - this.min) / 100;
        this.firstValue = Math.floor(targetValue)
      },

      onSliderClick(event) {
        if (this.dragging) return;
        const sliderOffsetLeft = this.$refs.slider.getBoundingClientRect().left;
          const sliderOffsetWidth = this.$refs.slider.getBoundingClientRect().width;
          this.setPosition((event.clientX - sliderOffsetLeft) / sliderOffsetWidth * 100);
        this.emitChange();
      },

      emitChange() {
        this.$nextTick(() => {
          this.$emit('change', this.value);
        });
      }
    },

    computed: {
      minValue() {
        return Math.min(this.firstValue, this.secondValue);
      },

      maxValue() {
        return Math.max(this.firstValue, this.secondValue);
      },

      barSize() {
        return `${ 100 * (this.firstValue - this.min) / (this.max - this.min) }%`;
      },

      barStart() {
        return '0%';
      },
      barStyle() {
        return {
            width: this.barSize
          }
      },
      handlerStyle() {
        return {
            left: this.barSize
          }
      }
    },

    mounted() {
      let valuetext;
      if (typeof this.value !== 'number' || isNaN(this.value)) {
          this.firstValue = this.min;
        } else {
          this.firstValue = Math.min(this.max, Math.max(this.min, this.value));
        }
        this.oldValue = this.firstValue;
        valuetext = this.firstValue;
      this.$el.setAttribute('aria-valuetext', valuetext);

      this.$el.setAttribute('aria-label', this.label ? this.label : `slider between ${this.min} and ${this.max}`);

    },

  };
</script>
