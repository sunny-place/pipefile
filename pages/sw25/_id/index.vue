<template lang="pug">
v-container(grid-list-md text-xs-center)
  h2 {{ $route.params.id }}
  p LV:{{characterLevel}}
  v-layout(row justify-space-between)
    v-flex(xs9 sm9 md9)
      v-text-field(label="キャラクター名" pa0 ma0)
    v-flex(xs3 sm3 md3)
      v-text-field(label="プレイヤー名" pa0 ma0)
  v-layout(row justify-space-between)
    v-flex(xs6 sm6 md6)
      v-text-field(label="種族" pa0 ma0)
    v-flex(xs3 sm3 md3)
      v-text-field(label="年齢" pa0 ma0)
    v-flex(xs3 sm3 md3)
      v-text-field(label="性別" pa0 ma0)
  status(:baseStatus="cData.status", :endStatus="endStatus", :calculatedStatus="calculatedStatus")
  classes(:pClass="cData.physicalClass", :mClass="cData.magicalClass", :oClass="cData.otherClass")
  skill(:lSkill="getLevelSkill", :oSkill="cData.otherSkill")
  weapon(:weapons="cData.weapons", :pClass="cData.physicalClass", :eStatus="endStatus")
  armor(:armor="cData.armor", :pMax="maxPhysicalLevel()", :eStatus="endStatus")
  accessory(:accessory="cData.accessory")

</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'
import Status from '~/components/parts/sw25/Status.vue'
import Classes from '~/components/parts/sw25/Classes.vue'
import Skill from '~/components/parts/sw25/Skill.vue'
import Weapon from '~/components/parts/sw25/Weapon.vue'
import Armor from '~/components/parts/sw25/Armor.vue'
import Accessory from '~/components/parts/sw25/Accessory.vue'

export default {
  components: {
    Logo,
    VuetifyLogo,
    Status,
    Classes,
    Skill,
    Weapon,
    Armor,
    Accessory,
  },
  data() {
    return {
      cData: {
        status: {
          gi  : 5, 
          tai : 9,
          shin: 15,
          a   : 1,
          b   : 2,
          c   : 3,
          d   : 4,
          e   : 5,
          f   : 6,
          aMod: 1,
          bMod: 2,
          cMod: 3,
          dMod: 4,
          eMod: 5,
          fMod: 6,
        },
        physicalClass: {
          fighter: {id: "fighter", label: "ファイター", value: 5},
          grappler: {id: "grappler", label: "グラップラー", value: 0},
          fencer: {id: "fencer", label: "フェンサー", value: 0},
          shooter: {id: "shooter", label: "シューター", value: 0},
        },
        magicalClass: {
          sorcerer: {id: "sorcerer", label: "ソーサラー", value: 1},
          conjurer: {id: "conjurer", label: "コンジャラー", value: 1},
          priest: {id: "priest", label: "プリースト", value: 1},
          magitech: {id: "magitech", label: "マギテック", value: 1},
        },
        otherClass: {
          scout: {id: "scout", label: "スカウト", value: 1},
          ranger: {id: "ranger", label: "レンジャー", value: 1},
          sage: {id: "sage", label: "セージ", value: 1},
        },
        levelSkill: {
          1:{name:"かばう", desc:"hogehoge"},
          3:{name:"全力攻撃", desc:"hogehoge"},
        },
        otherSkill: [
          {name: "格闘", desc:"hogehoge"}
        ],
        weapons: [
          {
            name: "ソード",
            usage: "１H",
            class: "fighter",
            requireStr: 1,
            accuracyMod: 1,
            power: 20,
            critical: 11,
            addDmg: 1,
          },
          {
            name: "ソード",
            usage: "１H",
            class: "fighter",
            requireStr: 1,
            accuracyMod: 1,
            power: 20,
            critical: 11,
            addDmg: 1,
          },
        ],
        armor: {
          body: {name: "プレートメイル", require: 0, avoid: 0, guard: 1},
          shield: {name: "バックラー", require: 0, avoid: 0, guard: 1},
          other:[{name: "おまもり", require: 0, avoid: 1, guard: 1}],
        },
        accessory: {
          head: {name: "髪飾り", desc: ""},
          face: {name: "お面", desc: ""},
          ear: {name: "イヤリング", desc: ""},
          neck: {name: "首輪", desc: ""},
          back: {name: "バックパック", desc: ""},
          rHand: {name: "指輪", desc: ""},
          lHand: {name: "腕輪", desc: ""},
          waist: {name: "ベルト", desc: ""},
          foot: {name: "くつ", desc: ""},
          other: {name: "そのた", desc: ""},
        },
      }
    }
  },
  computed: {
    endStatus() {
      return {
        dex: this.createStatusInfo("器用度",[this.cData.status.gi   , this.cData.status.a , this.cData.status.aMod]),
        agl: this.createStatusInfo("敏捷度",[this.cData.status.gi   , this.cData.status.b , this.cData.status.bMod]),
        str: this.createStatusInfo("筋力"  ,[this.cData.status.tai  , this.cData.status.c , this.cData.status.cMod]),
        vit: this.createStatusInfo("生命力",[this.cData.status.tai  , this.cData.status.d , this.cData.status.dMod]),
        int: this.createStatusInfo("知力"  ,[this.cData.status.shin , this.cData.status.e , this.cData.status.eMod]),
        mnd: this.createStatusInfo("精神力",[this.cData.status.shin , this.cData.status.f , this.cData.status.fMod]),
      }
    },
    calculatedStatus() {
      return {
        hpMax:     {label: "最大HP",   value: (this.characterLevel * 3 + this.endStatus.vit.value)} ,
        mpMax:     {label: "最大MP",   value: (this.totalMagicalLevel * 3 + this.endStatus.mnd.value)},
        move :     {label: "移動力",   value: (this.endStatus.agl.value)},
        dash :     {label: "全力移動", value: (this.endStatus.agl.value * 3)},
        vitResist: {label: "生命抵抗", value: (this.characterLevel + this.endStatus.vit.bonus)},
        mndResist: {label: "精神抵抗", value: (this.characterLevel + this.endStatus.mnd.bonus)},
      }
    },
    characterLevel() {
      var p = this.maxPhysicalLevel();
      var m = this.maxMagicalLevel();
      var o = this.maxOtherLevel();
      return Math.max(p,m,o);
      // return p;
    },
    totalMagicalLevel() {
      var v = 0;
      for(var key in this.cData.magicalClass) {
        v += this.cData.magicalClass[key].value;
      }
      return v;
    },
    getLevelSkill() {
      for(var i=1; i<=this.characterLevel; i+=2) {
        if(!(this.cData.levelSkill[i])) {
          this.cData.levelSkill[i] = {name: "", desc:""};
        }
      }
      return this.cData.levelSkill;
    }
  },
  methods: {
    createStatusInfo(lbl,vals) {
      var v = 0;
      vals.forEach(function(val,index,ar){
        v +=  (val==="" || isNaN(val)) ? 0 : parseInt(val);
      });
      return {
        label : lbl,
        value : v,
        bonus : Math.floor(v/6),
      }
    },
    maxPhysicalLevel() {
      return Math.max.apply(null,Object.values(this.cData.physicalClass).map(function(o){return o.value;}));
    },
    maxMagicalLevel() {
      return Math.max.apply(null,Object.values(this.cData.magicalClass).map(function(o){return o.value;}));
    },
    maxOtherLevel() {
      return Math.max.apply(null,Object.values(this.cData.otherClass).map(function(o){return o.value;}));
    },

  }
}

</script>

