## LuckyWheel props
  |参数 | 说明 | 类型 | 是否必传 |
  |:---------: | :-----------: | :------: | :------: |
  |prizeList | 奖品列表 | Array | 必传 |
  |asyncAjax | 异步发请求获取中奖请求函数 | Function | 必传 |
  |isPointer | 是否要指针旋转 | Boolean | 非必传 |
  
<br>

## LuckyWheel Methods
  |方法名 | 说明 | 参数 | 
  |:---------: | :------: | :------: | 
  |onSuccess | 旋转停止后的回到 | params 中奖的下标 |

<br>

```
<template>
  <div id="app">
    <LuckyWheel
      :prizeList="prizeList"
      :asyncAjax="asyncAjax"
      isPointer
      @onSuccess="onSuccess"
    />
  </div>
</template>

<script>
import LuckyWheel from './components/LuckyWheel'; // 引入组件
export default {
  name: 'app',
  data() {
    return {
      // 抽奖数据
      prizeList: [
        {
          name: '奖品1',
          img: require('./components/LuckyWheel/img/prize1.png')
        },
        {
          name: '奖品2',
          img: require('./components/LuckyWheel/img/prize2.png')
        },
        {
          name: '奖品3',
          img: require('./components/LuckyWheel/img/prize3.png')
        },
        {
          name: '奖品4',
          img: require('./components/LuckyWheel/img/prize4.png')
        },
        {
          name: '奖品5',
          img: require('./components/LuckyWheel/img/prize4.png')
        },
        {
          name: '奖品6',
          img: require('./components/LuckyWheel/img/prize5.png')
        },
        {
          name: '奖品7',
          img: require('./components/LuckyWheel/img/prize6.png')
        },
        {
          name: '谢谢惠顾',
          img: require('./components/LuckyWheel/img/prize7.png')
        }
      ]
    };
  },
  methods: {
    // 抽中后的回调
    onSuccess(index) {
      alert('恭喜您获得' + this.prizeList[index].name);
    },
    // 抽奖时发请求获取中奖下标
    asyncAjax() {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          let num = Math.floor(Math.random() * this.prizeList.length);
          resolve(num);
        }, 1000);
      });
    }
  },
  components: {
    LuckyWheel
  }
};
</script>

