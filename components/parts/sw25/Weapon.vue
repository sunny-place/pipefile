<template lang="pug">
  v-card
    v-toolbar(@click="show = !show")
      v-btn(icon )
        v-icon {{ show ? 'keyboard_arrow_down' : 'keyboard_arrow_up' }}
      v-toolbar-title 武器
    v-slide-y-transition
      v-container(fluid grid-list-lg v-show="show")
        v-layout(row wrap v-for="item in weapons")
          v-flex(xs2 sm2 md2 )
            v-text-field(label="名前" v-model="item.name")
          v-flex(xs1 sm1 md1 )
            v-select(label="用法" v-model="item.usage" :items="usageList")
          v-flex(xs2 sm2 md2 )
            v-select(label="対応技能" v-model="item.class" :items="Object.values(pClass)" item-text="label" item-value="id")
            //- v-text-field(label="" v-model="item.class")
          v-flex(xs1 sm1 md1 )
            v-text-field(label="必要筋力" v-model="item.requireStr")
          v-flex(xs1 sm1 md1 )
            v-text-field(label="命中補正" v-model="item.accuracyMod")
          v-flex(xs1 sm1 md1 )
            v-text-field(label="命中合計" :value="item.accuracyMod + baseAccuracy(item.class)")
          v-flex(xs1 sm1 md1 )
            v-text-field(label="威力" v-model="item.power")
          v-flex(xs1 sm1 md1 )
            v-text-field(label="C値" v-model="item.critical")
          v-flex(xs1 sm1 md1 )
            v-text-field(label="追加ダメ" v-model="item.addDmg")
          v-flex(xs1 sm1 md1 )
            v-text-field(label="追加ダメ計" :value="item.addDmg + baseAddDmg(item.class)")
          v-flex(xs12 sm12 md12 )
            power-table(:power="item.power" :accordion="true")
          v-flex(xs12 sm12 md12 )
            v-divider
</template>

<script>
import PowerTable from '~/components/parts/sw25/PowerTable.vue'
export default {
    props: ['weapons', 'pClass','eStatus'],
    data() {
      return {
        show: false,
        usageList: [
          "1H",
          "1H#",
          "2H"
        ],
      }
    },
    methods: {
      baseAccuracy(cls) {
        return this.eStatus.dex.bonus + (this.pClass[cls] ? this.pClass[cls].value : 0); 
      },
      baseAddDmg(cls) {
        return this.eStatus.str.bonus + (this.pClass[cls] ? this.pClass[cls].value : 0); 
      },
    },
    components: {
      PowerTable,
    }
  }
</script>

<style>

</style>
