<template>
  <div id="app">
    <input type="text" class="search" placeholder="search" v-model="search">
    <table class="t-city">
      <thead>
        <tr>
          <th v-for="(item, index) in title" :key="index">
            <button @click.prevent="foo(index)">{{ index }}</button>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in finalList" :key="index">
          <td v-for="(row, index) in item" :key="index">{{ row }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import city from "./city.json";
export default {
  name: "App",
  data() {
    return {
      info: Object.values(city),
      title: city[0],
      search: null,
      sortCol: {
        type: false,
        col: null
      }
    };
  },
  methods: {
    foo(index) {
      this.sortCol.type = !this.sortCol.type;
      this.sortCol.col = index;
      this.info.sort((a, b) => {
        let x = a[this.sortCol.col];
        let y = b[this.sortCol.col];
        if (this.sortCol.type) {
          if (x < y) {
            return -1;
          } else if (x > y) {
            return 1;
          } else {
            return 0;
          }
        } else if (!this.sortCol.type) {
          if (x > y) {
            return -1;
          } else if (x < y) {
            return 1;
          } else {
            return 0;
          }
        }
      });
    }
  },
  computed: {
    finalList() {
      let reg = new RegExp(this.search, "i");
      return this.info.filter(item => {
        if (this.search === null) return item;
        for (let key in item) {
          if (reg.test(item[key])) {
            return item;
          }
        }
      });
    }
  },
  mounted() {}
};
</script>

<style>
.btn {
  border: none;
  outline: none;
  padding: 10px 20px;
  font-size: 16px;
}
.t-city {
  border-collapse: collapse;
  width: 100%;
}
.t-city td {
  border: 1px solid rgb(134, 134, 134);
  padding: 0 10px;
}
.t-city thead th {
  padding: 0;
}
.t-city thead th button {
  border: none;
  box-sizing: border-box;
  background-color: rgb(12, 12, 12);
  color: white;
  height: 40px;
  width: 100%;
  padding: 0;
  outline: none;
  font-size: 16px;
  transition-property: background-color color;
  cursor: pointer;
  border-top: 3px solid rgba(0, 0, 255, 0);
}
.t-city thead th button:hover {
  transition-duration: 0.2s;
  background-color: rgb(34, 33, 33);
}
.t-city thead th button:focus {
  border-color: blue;
}
.t-city thead th button:active {
  color: blue;
}
.t-city tr:nth-child(even) {
  background-color: rgb(238, 233, 233);
}
.search {
  box-shadow: none;
  border: none;
  box-sizing: border-box;
  outline: none;
  width: 400px;
  height: 50px;
  margin: 20px 0;
  padding: 0 20px;
  font-size: 16px;
  border-bottom: 1px solid black;
}
.search::placeholder {
  text-transform: uppercase;
}
.search:focus {
  border-color: blue;
}
</style>
