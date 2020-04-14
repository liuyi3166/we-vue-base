<template>
  <div class="App">
    <div class="App-header"></div>
    <h1 class="App-title">{{ $t('common.welcome') }}</h1>
    <p class="App-intro">
      {{ $t('home.edit') }}
      <code>/src/views/Home.vue</code>
      {{ $t('home.saveReload') }}
    </p>
    <router-link to="/vueTemplateIntro" class="desc-link">{{
      $t('home.wlkVueTemplateIntro')
    }}</router-link>
    <div style="display:none;">{{ deviceInfo }}</div>
    <wv-radio-group v-model="radios" @change="changList">
      <wv-radio v-for="city in cities" :label="city" :key="city" :text="city" :placeholder="city" :content="city"></wv-radio>
    </wv-radio-group>
  </div>
</template>

<script>
import { mapState, mapActions } from 'vuex';
import wvRadio from '@/components/WvRadio/radio.vue'
import wvRadioGroup from '@/components/WvRadio/radio-group.vue'
export default {
  data() {
    return {
      label:'选择',
      placeholder: '请选择数据',
      checkVaule: true,
      radios:'北京',
      checkList: ['北京','上海'],
      cities:['北京','上海','广州','深圳'],
      checked: true
    };
  },
  components: {
    wvRadio,
    wvRadioGroup
  },
  created() {
    window.HWH5.navTitle({ title: 'Hello World!' });
    this.getDeviceInfo();
  },
  computed: {
    ...mapState(['deviceInfo'])
  },
  methods: {
    ...mapActions(['getDeviceInfo']),
    click(event){
      console.log('>>>>>>>>>>>>>>>>>>>>>>>>>')
    },
    changList(data){
      console.log('changList:', data, this.radios)
    }
  }
};
</script>

<style lang="less" scoped>
.App {
  padding: 10px;

  .App-logo {
    margin: 20px 0;
    text-align: center;
  }

  .App-header {
    height: 90px;
    text-align: center;
  }

  .App-title {
    margin-bottom: 30px;
    font-size: 1.5em;
    text-align: center;
  }

  .App-intro {
    margin-bottom: 10px;
    // font-size: large;
    text-align: center;
  }

  .desc-link {
    display: block;
    line-height: 2.2em;
    margin: 0 auto;

    color: #2b88e6;
    font-size: 14px;
    text-align: center;
  }
  code {
    padding: 2px 4px;
    color: #c0341d;
    background-color: #fbe5e1;
    border-radius: 4px;
  }

  .develop-doc {
    h3 {
      margin-bottom: 5px;
    }

    ul {
      list-style: circle;
      padding-left: 30px;
    }

    li {
      margin-bottom: 5px;
      color: #2b88e6;
      cursor: pointer;
    }
  }
}
</style>
