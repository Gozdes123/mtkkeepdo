<template>
  <div class="searchbar">
    <input
      type="search"
      v-model.lazy.trim="keyWord"
      placeholder="請輸入要查詢的東西"
      class="inputext"
      @keyup.enter="search()"
    />
    <button @click="search()" class="">搜尋</button>
    <button @click="refresh()" class="">重整</button>
    <date-picker
      v-model:value="timetarget"
      class="time"
      @keyup.enter="DateSearch()"
    ></date-picker>
    <button @click="DateSearch()">
      <i class="fa-solid fa-magnifying-glass"></i>
    </button>
  </div>
  <div class="flex">
    <div class="w-[25%] text-center">
      <span class="up"
        ><button @click="MissionSortList(datekeyup)">
          <i class="fa-solid fa-angles-up up"></i></button></span
      >Mission Name
      <span class="down"
        ><button @click="MissionSortList(datekeydown)">
          <i class="fa-solid fa-angles-down down"></i></button
      ></span>
    </div>
    <div class="w-[25%] text-center">
      <span class="up"
        ><button @click="RocketSortList(datekeyup)">
          <i class="fa-solid fa-angles-up up"></i></button
      ></span>
      Rocket Name
      <span class="down"
        ><button @click="RocketSortList(datekeydown)">
          <i class="fa-solid fa-angles-down down"></i></button
      ></span>
    </div>
    <div class="w-[25%] text-center">
      <span class="up"
        ><button @click="RocketTypeSortList(datekeyup)">
          <i class="fa-solid fa-angles-up up"></i></button
      ></span>
      Rocket Type
      <span class="down"
        ><button @click="RocketTypeSortList(datekeydown)">
          <i class="fa-solid fa-angles-down down"></i></button
      ></span>
    </div>
    <div class="w-[25%] text-center">
      <span class="up"
        ><button @click="DateSortList(datekeydown)">
          <i class="fa-solid fa-angles-up up"></i></button
      ></span>
      Launch Date
      <span class="down"
        ><button @click="DateSortList(datekeyup)">
          <i class="fa-solid fa-angles-down down"></i></button
      ></span>
    </div>
  </div>
  <div v-for="(datas, index) in PaginatedData" :key="index" class="flex">
    <div class="w-[25%] text-center">{{ datas.mission_name }}</div>
    <div class="w-[25%] text-center">{{ datas.rocket.rocket_name }}</div>
    <div class="w-[25%] text-center">{{ datas.rocket.rocket_type }}</div>
    <div class="w-[25%] text-center">{{ datas.launch_date_local }}</div>
  </div>
  <div v-if="PaginatedData[19] == null">
    <div
      v-for="(div, index) in 20 - PaginatedData.length"
      :key="index"
      class="flex"
    >
      <div class="w-[25%] text-center">&emsp;</div>
      <div class="w-[25%] text-center">&emsp;</div>
      <div class="w-[25%] text-center">&emsp;</div>
      <div class="w-[25%] text-center">&emsp;</div>
    </div>
  </div>
  <div class="text-center">
    <button @click="PrePage()">
      <i class="fa-solid fa-circle-arrow-left"></i>
    </button>
    <button
      v-for="(page, index) in TotalPage"
      :key="index"
      :style="{ backgroundColor: index == this.zero ? '#fcaa3f' : 'white' }"
      class="w-[20px]"
      @click="ClickPage(index)"
    >
      {{ index + 1 }}
    </button>
    <button @click="NextPage()">
      <i class="fa-solid fa-circle-arrow-right"></i>
    </button>
  </div>
</template>
<script>
import DatePicker from "vue-datepicker-next";
import "vue-datepicker-next/index.css";
import dayjs from "dayjs";
export default {
  created() {
    fetch("https://api.spacex.land/rest/launches")
      .then((response) => {
        return response.json();
      })
      .then((response) => {
        response.map(function (array) {
          array.launch_date_local = dayjs(array.launch_date_local).format(
            "YYYY/MM/DD"
          );
        });
        this.newarray = response;
        this.newarray2 = JSON.parse(JSON.stringify(response));
      })
      .catch((error) => {
        console.log("沒接到資料" + error);
      });
  },
  components: { DatePicker },
  data() {
    return {
      zero: 0,
      per: 20,
      keyWord: "",
      datekeyup: "up",
      datekeydown: "down",
      newarray: [],
      newarray2: [],
      timetarget: null,
      time: null,
    };
  },
  methods: {
    refresh() {
      this.newarray = this.newarray2.map((array) => {
        return array;
      });
      this.keyWord = "";
      this.zero = 0;
      this.timetarget = null;
    },
    PrePage() {
      if (this.zero <= 0) {
        this.zero = 0;
      } else {
        this.zero--;
      }
    },
    ClickPage(num) {
      this.zero = num;
    },
    NextPage() {
      if (this.zero >= Math.floor(this.newarray.length / this.per)) {
        this.zero = Math.floor(this.newarray.length / this.per);
      } else {
        this.zero++;
      }
    },
    MissionSortList(key) {
      if (key == "up") {
        this.newarray.sort((a, b) => {
          return a.mission_name.localeCompare(b.mission_name, "en", {
            numeric: true,
          });
        });
      } else if (key == "down") {
        this.newarray.sort((a, b) => {
          return b.mission_name.localeCompare(a.mission_name, "en", {
            numeric: true,
          });
        });
      }
    },
    RocketSortList(key) {
      if (key == "up") {
        this.newarray.sort((a, b) => {
          return a.rocket.rocket_name.localeCompare(
            b.rocket.rocket_name,
            "en",
            {
              numeric: true,
            }
          );
        });
      } else if (key == "down") {
        this.newarray.sort((a, b) => {
          return b.rocket.rocket_name.localeCompare(
            a.rocket.rocket_name,
            "en",
            {
              numeric: true,
            }
          );
        });
      }
    },
    RocketTypeSortList(key) {
      if (key == "up") {
        this.newarray.sort((a, b) => {
          return a.rocket.rocket_type.localeCompare(
            b.rocket.rocket_type,
            "en",
            {
              numeric: true,
            }
          );
        });
      } else if (key == "down") {
        this.newarray.sort((a, b) => {
          return b.rocket.rocket_type.localeCompare(
            a.rocket.rocket_type,
            "en",
            {
              numeric: true,
            }
          );
        });
      }
    },
    DateSortList(key) {
      if (key == "up") {
        this.newarray.sort((a, b) => {
          let newa = dayjs(a.launch_date_local).format("YYYY-MM-DD");
          let newb = dayjs(b.launch_date_local).format("YYYY-MM-DD");
          return newa < newb ? 1 : -1;
        });
      } else if (key == "down") {
        this.newarray.sort((a, b) => {
          let newa = dayjs(a.launch_date_local).format("YYYY-MM-DD");
          let newb = dayjs(b.launch_date_local).format("YYYY-MM-DD");
          return newb < newa ? 1 : -1;
        });
      }
    },
    search() {
      if (this.keyWord !== "") {
        this.newarray = this.newarray2.filter(
          (searchResult) =>
            searchResult.launch_date_local.includes(this.keyWord) ||
            searchResult.launch_date_local.includes(
              this.keyWord.replace(/-/g, "/")
            ) ||
            searchResult.rocket.rocket_type
              .toUpperCase()
              .includes(this.keyWord.toUpperCase()) ||
            searchResult.rocket.rocket_name
              .toUpperCase()
              .includes(this.keyWord.toUpperCase()) ||
            searchResult.mission_name
              .toUpperCase()
              .includes(this.keyWord.toUpperCase())
        );
        this.zero = 0;
      } else {
        this.newarray = this.newarray2.map((array) => {
          return array;
        });
      }
    },
    DateSearch() {
      this.time = dayjs(this.timetarget).format("YYYY/MM/DD");
      this.newarray = this.newarray2.filter((searchResult) => {
        return searchResult.launch_date_local.includes(this.time);
      });
      this.zero = 0;
    },
  },
  computed: {
    PaginatedData() {
      let start = this.zero * 20;
      let end = start + 20;
      return this.newarray.slice(start, end);
    },
    TotalPage() {
      return Math.ceil(this.newarray.length / this.per);
    },
  },
};
</script>

<style></style>
