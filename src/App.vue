<template>
  <div id="app">
    <h1>{{ title }}</h1>
    <hr />
    <div>
      <h2>添加课程</h2>
      <div>
        <label for>课程名称:</label>
        <input type="text" v-model="courseInfo.name" />
      </div>
      <div>
        <label for>课程价格:</label>
        <!-- 这里使用v-model来做一个数据的双向绑定,把数组动态双向绑定到course里面 -->
        <input type="text" v-model="courseInfo.price" />
      </div>
      <div>
        <button @click="addCourseToList">添加课程到列表</button>
      </div>
    </div>
    <dir>
      <h2>课程列表</h2>
      <table>
        <tr>
          <th>课程名称</th>
          <th>课程价格</th>
          <th>操作</th>
        </tr>
        <tr v-for="(item, index) in courseList" :key="item.id">
          <td>{{item.name}}</td>
          <td>{{item.price}}</td>
          <td>
            <button @click="addCourseToCart(index)">添加到购物车</button>
          </td>
        </tr>
      </table>
    </dir>
    <cart :courseItem="courseItem" @removeItem="remove" @minusItem="minusItem" @addItem="addItem" />
  </div>
</template>

<script>
import cart from "./components/Cart";
export default {
  name: "app",
  components: {
    cart
  },
  methods: {
    addCourseToList() {
      this.courseList.push(this.courseInfo);
    },
    addCourseToCart(index) {
      let item = this.courseList[index];
      let isHasCourse = this.courseItem.find(x => x.id === item.id);
      if (isHasCourse) {
        isHasCourse.number++;
      } else {
        this.courseItem.push({ ...item, number: 1, isActive: false });
      }
    },
    remove(index) {
      this.courseItem.splice(index, 1);
    },
    minusItem(index) {
      this.courseItem[index].number--;
    },
    addItem(index) {
      this.courseItem[index].number++;
    }
  },
  data() {
    return {
      title: "购物车",
      courseInfo: {
        name: "",
        price: ""
      },
      courseItem: [],
      courseList: [
        { name: "计算机网络", price: 9999, id: 1 },
        { name: "深度学习", price: 8888, id: 2 }
      ]
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>
