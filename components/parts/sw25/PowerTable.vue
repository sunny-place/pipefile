<template lang="pug">
  v-card
    p(v-for="item in powerTable") {{item}}
</template>

<script>

export default {
    props: ['power'],
    data() {
      return {
        point: [
          [10,23,32,39,51,74],
          [5,11,24,30,36,41,51,56,59],
          [3,8,13,24,31,37,42,50,53,55],
          [4,9,14,22,27,35,43,45,49,57,65,71],
          [6,12,17,21,29,33,40,44,52,58,62,68,72,75],
          [1,7,15,18,23,26,34,38,47,48,60,64,66,69,76,79],
          [2,10,16,19,25,28,32,39,46,54,59,63,67,70,74,78,80],
          [2,5,11,16,20,26,30,36,41,46,53,56,61,63,70,73,77,80],
          [4,8,13,19,20,28,34,37,42,49,50,55,61,65,67,73,77,78],
          [3,7,9,14,19,20,31,35,38,43,47,54,57,60,66,71,73,77,78],
        ],
        max: 80,
      }
    },
    computed: {
      powerTable() {
        var pow = (this.power==="" || isNaN(this.power)) ? 0 : parseInt(this.power);
        var result = [0,0,0,1,2,2,3,3,4,4];
        var idx = 0;
        this.point.forEach(column => {
          column.forEach(element => {
            if(this.power >= element) {
              result[idx] += 1;
            }
          });
          idx += 1;
        });
        return result;
      }
    },
    components: {
    }
  }
</script>

<style>

</style>
