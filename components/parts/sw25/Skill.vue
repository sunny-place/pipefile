<template lang="pug">
  v-card
    v-toolbar(@click="show = !show")
      v-btn(icon )
        v-icon {{ show ? 'keyboard_arrow_down' : 'keyboard_arrow_up' }}
      v-toolbar-title 戦闘特技
    v-slide-y-transition
      v-container(fluid grid-list-lg v-show="show")
        v-subheader レベル毎習得
        v-layout(row wrap v-for="(item,idx) in lSkill")
          v-flex(xs3 sm3 md3 )
            v-text-field(:label="'レベル'+idx" v-model="item.name")
          v-flex(xs9 sm9 md9 )
            v-text-field(label="説明" v-model="item.desc")
        v-subheader 自動習得など
        v-layout(row wrap align-center)
          v-flex(xs3 sm3 md3 v-for="item in oSkill")
            v-text-field(label="" v-model="item.name")
          v-btn(icon @click="add_oSkill")
            v-icon(large) {{'add_box'}}

</template>

<script>
export default {
    props: ['lSkill','oSkill'],
    data() {
      return {
        show: false
      }
    },
    methods: {
      add_oSkill() {
        this.oSkill.push({name:"", desc:""});
      }
    },
    components: {
    }
  }
</script>

<style>

</style>
