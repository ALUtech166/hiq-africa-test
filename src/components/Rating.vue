<template>
   <div>
      <small>{{ rate }}</small>
      <span class="text-yellow-500">
         <template v-for="star in starArr" :key="star">
            <i :class="setStarIcon(star)" ></i>
         </template>
      </span>
   </div>
</template>

<script>
import { computed } from 'vue';

export default {
  name: 'Rating',
  props: {
    rate: {
      type: Number,
      default: 0,
    },
  },
  setup(props) {
    const starArr = computed(() => {
      const maxStar = 5;
      const mainNum = props.rate * 0.5;
      const lowerNum = Math.floor(mainNum);
      const upperNum = lowerNum + 1;
      const arr = [];

      for (let i = 1; i <= lowerNum; i++) {
        arr.push('full');
      }

      if (mainNum > lowerNum && mainNum < upperNum) {
        arr.push('half');
      }

      for (let i = arr.length + 1; i <= maxStar; i++) {
        arr.push('empty');
      }

      return arr;
    });

    const setStarIcon = (str) => {
      if (str === 'full') {
        return 'fas fa-star';
      } else if (str === 'half') {
        return 'fas fa-star-half';
      } else {
        return 'far fa-star';
      }
    };

    return {
      starArr,
      setStarIcon,
    };
  },
};
</script>

<style lang="scss">
   
   
</style>