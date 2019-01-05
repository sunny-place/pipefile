<template lang="pug">
  v-card
    v-toolbar(@click="show = !show")
      v-btn(icon )
        v-icon {{ show ? 'keyboard_arrow_down' : 'keyboard_arrow_up' }}
      v-toolbar-title 防具
    v-slide-y-transition
      v-container(fluid grid-list-lg v-show="show")
        v-layout(row wrap)
          v-flex(xs6 sm6 md6)
            v-layout(row wrap)
              v-flex(xs12 sm12 md12)
                p.mb-0.text-md-left.text-md-left.text-sm-left.text-xs-left 鎧
              v-flex(xs6 sm6 md6)
                v-text-field(label="名前" v-model="armor.body.name")
              v-flex(xs2 sm2 md2)
                v-text-field(label="必要筋力" v-model="armor.body.require")
              v-flex(xs2 sm2 md2)
                v-text-field(label="回避修正" v-model="armor.body.avoid")
              v-flex(xs2 sm2 md2)
                v-text-field(label="防護点" v-model="armor.body.guard")
          v-flex(xs6 sm6 md6)
            v-layout(row wrap)
              v-flex(xs12 sm12 md12)
                p.mb-0.text-md-left.text-md-left.text-sm-left.text-xs-left 盾
              v-flex(xs6 sm6 md6)
                v-text-field(label="名前" v-model="armor.shield.name")
              v-flex(xs2 sm2 md2)
                v-text-field(label="必要筋力" v-model.number="armor.shield.require")
              v-flex(xs2 sm2 md2)
                v-text-field(label="回避修正" v-model.number="armor.shield.avoid")
              v-flex(xs2 sm2 md2)
                v-text-field(label="防護点" v-model.number="armor.shield.guard")
          v-flex(xs12 sm12 md12)
            v-layout(row wrap)
              v-flex(xs12 sm12 md12)
                p.mb-0.text-md-left.text-md-left.text-sm-left.text-xs-left その他
            v-layout(row wrap)
              v-flex(xs6 sm6 md6 v-for="item in armor.other")
                v-layout(row wrap )
                  v-flex(xs6 sm6 md6)
                    v-text-field(label="名前" v-model="item.name")
                  v-flex(xs2 sm2 md2)
                    v-text-field(label="必要筋力" v-model.number="item.require")
                  v-flex(xs2 sm2 md2)
                    v-text-field(label="回避修正" v-model.number="item.avoid")
                  v-flex(xs2 sm2 md2)
                    v-text-field(label="防護点" v-model.number="item.guard")
              v-btn(icon @click="add_other")
                v-icon(large) {{'add_box'}}
          v-flex(xs6 sm6 md6)
            status-card(:list="[{label:'合計回避力',value:totalAvoid}]")
          v-flex(xs6 sm6 md6)
            status-card(:list="[{label:'合計防護点',value:totalGuard}]")
</template>

<script>
import StatusCard from '~/components/parts/common/StatusCard.vue'
export default {
    props: ['armor', 'pMax', 'eStatus'],
    data() {
      return {
        show: false,
      }
    },
    computed: {
      totalAvoid() {
        var ret = this.eStatus.agl.bonus + this.pMax;
        ret += this.armor.body.avoid + this.armor.shield.avoid;
        this.armor.other.forEach(item => {
          ret += item.avoid;
        });
        return ret;
      },
      totalGuard() {
        var ret = this.armor.body.guard + this.armor.shield.guard;
        this.armor.other.forEach(item => {
          ret += item.guard;
        });
        return ret;
      },
    },
    methods: {
      add_other() {
        this.armor.other.push({name: "", require:0, avoid: 0, guard: 0});
      }
    },
    components: {
      StatusCard,
    }
  }
</script>

<style>

</style>
