<template>
  <div>
    <table>
      <tr>
        <td>勾选</td>
        <td>课程名称</td>
        <td>课程价格</td>
        <td>数量</td>
        <td>价格</td>
      </tr>
      <tr v-for="(item, index) in courseItem" :key="index">
        <td>
          <input type="checkbox" v-model="item.isActive" />
        </td>
        <td>{{item.name}}</td>
        <td>{{item.price}}</td>
        <td>
          <button @click="minus(index)">-</button>
          {{item.number}}
          <button @click="add(index)">+</button>
        </td>
        <td>{{item.price * item.number}}</td>
      </tr>
      <tr>
        <td></td>
        <td colspan="2">{{isActiveCourse}}/{{allCourseList}}</td>
        <td colspan="2">{{allPrice}}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  // vue里面推荐单相数据流，我们一般不会在子组件里面修改props传递过来的值
  // 但是其实想改还是可以的
  props: ["courseItem"],
  methods: {
    // 这里的这种写法不推荐
    minus(index) {
      let { number } = this.courseItem[index];
      if (number > 1) {
        this.$emit("minusItem", index);
      } else {
        if (window.confirm("确定要删除吗?")) {
          this.$emit("removeItem", index);
        }
      }
    },
    add(index) {
      this.$emit("addItem", index);
    }
  },
  // 计算数据
  computed:{
     isActiveCourse() {
        return this.courseItem.filter(item=>item.isActive).length
     },
     allCourseList() {
        return this.courseItem.length;
     },
     allPrice() {
        let num = 0;
        this.courseItem.forEach(item=>{
          if(item.isActive) {
            num += item.price * item.number;
          }
        })
        return num;
     }
  }
};
</script>

<style>
</style>