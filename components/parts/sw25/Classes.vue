<template lang="pug">
  v-card
    v-toolbar(@click="show = !show")
      v-btn(icon )
        v-icon {{ show ? 'keyboard_arrow_down' : 'keyboard_arrow_up' }}
      v-toolbar-title 技能
    v-slide-y-transition
      v-container(fluid grid-list-lg v-show="show")
        v-subheader 戦士系技能
        v-layout(row wrap )
          v-flex(xs3 sm3 md3 v-for="item in pClass")
            v-text-field(:label="item.label" v-model.number="item.value")
        v-subheader 魔法系技能
        v-layout(row wrap )
          v-flex(xs3 sm3 md3 v-for="item in mClass")
            v-text-field(:label="item.label" v-model.number="item.value")
        v-subheader その他技能
        v-layout(row wrap )
          v-flex(xs3 sm3 md3 v-for="item in oClass")
            v-text-field(:label="item.label" v-model.number="item.value")

</template>

<script>
import StatusCard from '~/components/parts/common/StatusCard.vue'
export default {
    props: ['pClass','mClass', 'oClass'],
    data() {
      return {
        show: false
      }
    },
    components: {
      StatusCard,
    }
  }
</script>

<style>

</style>
