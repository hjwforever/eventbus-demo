<template>
  <div class="hello">
    <span>num: {{ num }}</span
    ><button @click="num = 0">Reset</button><br />
    <span>count: {{ count }}</span> <br /><br />
    <button @click="on">Subscribe One</button><br /><br />
    <button @click="off">UnSubscribe One (the same function)</button
    ><br /><br />
    <button @click="offAnother">UnSubscribe One (another function)</button
    ><br /><br />
    <button @click="offAll">UnSubscribe All</button><br /><br />
    <button @click="emit">Trigger</button>
    <!-- | <button @click="$bus.$emit('event')">Click Me 2</button><br /> -->
  </div>
</template>

<script>
export default {
  name: "EventBusDemo",
  data() {
    return {
      num: 0,
      count: 0, // 统计事件订阅的方法数目
    };
  },
  mounted() {
    // this.on();
  },
  beforeDestory() {
    // this.off();
  },
  methods: {
    // 定义函数方法
    myEvent(data, callback) {
      this.num++;
      console.log("$emit data", data);
      console.log("num = ", this.num);
      callback(this.num);
    },
    // 对事件添加一次订阅, 即添加一个监听器
    on() {
      // 通过eventBus订阅事件'event'绑定一个'myEvent'方法， 即添加一个监听器
      // 注意，同一方法可添加多次，并且如果没有被取消订阅的话，信息触发时全都会被一一执行
      this.$bus.$on("event", this.myEvent);
      // 统计数加一
      this.count++;
      console.log("count = ", this.count);
    },
    // 取消事件的一次订阅, 即移除一个监听器
    off() {
      // 取消对事件'event'的一个名为'myEvent'方法的订阅
      // 注意, 如果已经添加了多次，则只可取消一次，即$off这种双参数用法只可移除一个监听器
      this.$bus.$off("event", this.myEvent);
      // 统计数减一
      this.count = Math.max(this.count - 1, 0);
      console.log("count = ", this.count);
    },
    offAnother() {
      // 对事件'event'解绑一个监听器函数，
      // 但下面是新建的一个匿名函数，则该匿名函数当然也不与之前任何已绑定的监听器函数相等，
      // 事件总线eventBus当然也就找不到相应的已绑定监听器进行解绑， 即使其代码内容与之前的函数一致
      this.$bus.$off("event", (data, callback) => {
        this.num++;
        console.log("$emit data", data);
        console.log("num = ", this.num);
        // 统计数减一
        this.count = Math.max(this.count - 1, 0);
        console.log("count = ", this.count);
        callback(this.num);
      });
    },
    // 取消事件的所有订阅, 即移除所有监听器
    offAll() {
      // 取消对事件'event'的所有订阅
      // 即$off这种一个参数的用法, 只可移除指定事件名的所有监听器
      this.$bus.$off("event");
      // 统计数归零
      this.count = 0;
      console.log("count = ", this.count);
    },
    // 发送事件信号
    emit() {
      this.$bus.$emit("event", this.num, (res) => {
        console.log("[event callback]: res", res);
      });
    },
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>