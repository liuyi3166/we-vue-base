<template>
  <a href="javascript:;" @click="click" class="weui-grid">
      <div style="margin:auto; text-align: center;">
          <i :class="[classSize, classType]"></i>
      </div>
      <p class="weui-grid__label">{{label}}</p>
  </a>
</template>
<script>
export default {
  name: 'wv-button',
  props: {
    label:{
      type: String,
      default: ''
    },
    type:{
      type: String,
      default: ''
    },
    size:{
      type: String,
      default: 'medium' //medium / small / mini
    }
  },
  data() {
    return {
      classSize: this.size === 'mini' ? 'icon-12' : this.size === 'medium' ? 'icon-16' : 'icon-24',
      classType: this.size === 'mini' ? 'icon-12-' + this.type : this.size === 'medium' ? 'icon-16-' + this.type : 'icon-24-' + this.type
    };
  },
  created(){
    console.log(this.type)
  },
   methods: {
    click(event){
      this.$emit('click',event)
    }
  }
};
</script>