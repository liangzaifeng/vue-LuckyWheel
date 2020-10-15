<template>
  <div class="wheel-wrap" @transitionend="transitionend">
    <div
      class="wheel"
      :style="{ transform, transition: `transform ${duration}s` }"
    >
      <div class="prize">
        <div
          class="prize-item"
          v-for="(item, index) in prizeList"
          :key="index"
          :style="{
            transform: `rotate(${(360 / prizeList.length) * index +
              21}deg) translateY(-1.3rem)`
          }"
        >
          <img :src="item.img" alt="" />
          <p>{{ item.name }}</p>
        </div>
      </div>
    </div>
    <div
      class="pointer"
      @click="handleClick"
      :style="{
        transform: pointerTransform,
        transition: `transform ${duration}s`
      }"
    >
      <img src="./img/pointer.png" alt="" />
    </div>
  </div>
</template>
<script>
export default {
  name: 'LuckWheel',
  props: {
    // 奖品列表
    prizeList: {
      type: Array,
      default: () => []
    },
    // 异步发请求获取中奖请求
    asyncAjax: {
      type: Function,
      default: null
    },
    // 是否要指针旋转
    isPointer: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      transform: '', // 转盘旋转的样式
      pointerTransform: '', // 指针旋转样式
      cycles: 4, // 固定旋转几圈
      pointercycles: 8, // 指针固定旋转几圈
      preDeg: 0, // 上一次的旋转角度
      flag: true, // 节流
      winningIndex: undefined, // 中间的索引
      duration: 3 // 旋转时间
    };
  },
  methods: {
    // 开始抽奖
    async handleClick() {
      if (!this.flag) return;
      this.flag = false;
      // 如果有异步请求则执行, 没有则生成需要选中的随机数索引
       let winningIndex = this.asyncAjax
        ? await this.asyncAjax()
        : Math.floor(Math.random() * this.prizeList.length - 1)
      console.log('winningIndex:' + winningIndex);
      this.winningIndex = winningIndex;
      if (this.isPointer) {
        this.pointerTransform = `rotate(${this.countDeg(winningIndex)}deg)`;
      } else {
        let randomIndex = this.prizeList.length - 1 - winningIndex;
        this.transform = `rotate(${this.countDeg(randomIndex)}deg)`;
      }
    },

    // 计算圈数
    countDeg(index) {
      // 旋转圈数
      let cyclesDeg = this.isPointer ? this.pointercycles : this.cycles;
      cyclesDeg *= 360;

      // 旋转角度
      let sumDeg =
        (360 / this.prizeList.length) * index + 21 + cyclesDeg + this.preDeg;

      // 初始化到原始点
      this.preDeg =
        (360 / this.prizeList.length) * (this.prizeList.length - index) -
        21 +
        sumDeg;
      return sumDeg;
    },
    transitionend() {
      this.flag = true;
      this.$emit('onSuccess', this.winningIndex);
    }
  }
};
</script>
<style>
html {
  font-size: 13.3333vw;
}
</style>
<style lang="scss" scoped>
.wheel-wrap {
  position: relative;
  display: inline-block;
  overflow: hidden;

  .wheel {
    position: relative;
    width: 6.26rem;
    height: 6.26rem;
    background: url('./img/dial.png') no-repeat center top;
    background-size: 100%;

    .prize {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      .prize-item {
        position: absolute;
        width: 1.6rem;
        bottom: 0;
        left: -0.8rem;
        text-align: center;
        transform-origin: bottom center;

        p {
          font-size: 0.12rem;
          margin: 0;
        }
        img {
          width: 1.3rem;
          height: 0.8rem;
          vertical-align: middle;
        }
      }
    }
  }
}

.pointer {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -0.88rem;
  margin-top: -0.9rem;
  z-index: 2;
  width: 1.67rem;
  height: 1.67rem;

  img {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 1.67rem;
    height: 1.83rem;
  }
}
</style>
