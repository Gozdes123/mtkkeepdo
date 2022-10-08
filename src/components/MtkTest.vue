<template>
  <div class="text-center mb-[10px] mt-[10px]">
    <button @click="refresh()" class="">
      <i
        class="fa-solid fa-arrows-rotate text-[20px] pr-[20px] text-[#FFD762]"
      ></i>
    </button>
    <input
      type="text"
      v-model.lazy.trim="keyWord"
      placeholder="請輸入要查詢的東西"
      class="border-solid border-2 border-[#ccc] rounded-lg"
      @keyup.enter="search()"
    />
    <button @click="search()" class="bg-[#FFD762] rounded-lg ml-[10px]">
      <p class="text-[white]">
        <i class="fa-solid fa-magnifying-glass"></i>&ensp;搜尋
      </p>
    </button>
    <date-picker
      v-model:value="timetarget"
      class="pr-[20px] pl-[20px]"
      @keyup.enter="DateSearch()"
    ></date-picker>
    <button @click="DateSearch()" class="bg-[#FFD762] rounded-lg">
      <p class="text-[white]">
        <i class="fa-solid fa-magnifying-glass"></i>&ensp;搜尋
      </p>
    </button>
  </div>
  <div class="flex">
    <div
      class="w-[25%] text-center bg-[#FFD762] rounded-full pb-[5px] pt-[5px] flex"
    >
      <div class="w-[33%]">
        <button @click="MissionSortList(datekeyup)">
          <i class="fa-solid fa-angles-up up"></i>
        </button>
      </div>
      <div class="w-[33%]">Mission Name</div>
      <div class="w-[33%]">
        <button @click="MissionSortList(datekeydown)">
          <i class="fa-solid fa-angles-down down"></i>
        </button>
      </div>
    </div>
    <div
      class="w-[25%] text-center bg-[#FFD762] rounded-full pb-[5px] pt-[5px] flex"
    >
      <div class="w-[33%]">
        <button @click="RocketSortList(datekeyup)">
          <i class="fa-solid fa-angles-up up"></i>
        </button>
      </div>
      <div class="w-[33%]">Rocket Name</div>
      <div class="w-[33%]">
        <button @click="RocketSortList(datekeydown)">
          <i class="fa-solid fa-angles-down down"></i>
        </button>
      </div>
    </div>
    <div
      class="w-[25%] text-center bg-[#FFD762] rounded-full pb-[5px] pt-[5px] flex"
    >
      <div class="w-[33%]">
        <button @click="RocketSortList(datekeyup)">
          <i class="fa-solid fa-angles-up up"></i>
        </button>
      </div>
      <div class="w-[33%]">Rocket Type</div>
      <div class="w-[33%]">
        <button @click="RocketSortList(datekeydown)">
          <i class="fa-solid fa-angles-down down"></i>
        </button>
      </div>
    </div>
    <div
      class="w-[25%] text-center bg-[#FFD762] rounded-full pb-[5px] pt-[5px] flex"
    >
      <div class="w-[33%]">
        <button @click="RocketSortList(datekeyup)">
          <i class="fa-solid fa-angles-up up"></i>
        </button>
      </div>
      <div class="w-[33%]">Launch Date</div>
      <div class="w-[33%]">
        <button @click="RocketSortList(datekeydown)">
          <i class="fa-solid fa-angles-down down"></i>
        </button>
      </div>
    </div>
  </div>
  <div v-for="(datas, index) in PaginatedData" :key="index" class="flex">
    <div
      class="w-[25%] text-center mt-[10px] border-solid border-2 rounded-full bg-[#f9c27bb8] border-[#f9c27bb8]"
    >
      {{ datas.mission_name }}
    </div>
    <div
      class="w-[25%] text-center mt-[10px] border-solid border-2 rounded-full bg-[#f9c27bb8] border-[#f9c27bb8]"
    >
      {{ datas.rocket.rocket_name }}
    </div>
    <div
      class="w-[25%] text-center mt-[10px] border-solid border-2 rounded-full bg-[#f9c27bb8] border-[#f9c27bb8]bg-[#f9c27bb8] border-[#f9c27bb8]"
    >
      {{ datas.rocket.rocket_type }}
    </div>
    <div
      class="w-[25%] text-center mt-[10px] border-solid border-2 rounded-full bg-[#f9c27bb8] border-[#f9c27bb8]"
    >
      {{ datas.launch_date_local }}
    </div>
  </div>
  <div v-if="PaginatedData[19] == null">
    <div
      v-for="(div, index) in 20 - PaginatedData.length"
      :key="index"
      class="flex"
    >
      <div
        class="w-[25%] text-center mt-[10px] border-solid border-2 border-white"
      >
        &emsp;
      </div>
      <div
        class="w-[25%] text-center mt-[10px] border-solid border-2 border-white"
      >
        &emsp;
      </div>
      <div
        class="w-[25%] text-center mt-[10px] border-solid border-2 border-white"
      >
        &emsp;
      </div>
      <div
        class="w-[25%] text-center mt-[10px] border-solid border-2 border-white"
      >
        &emsp;
      </div>
    </div>
  </div>
  <div class="text-center mt-[20px] flex">
    <div class="w-[33%]">
      <button @click="PrePage()">
        <i class="fa-solid fa-circle-arrow-left text-[20px] text-[#fcaa3f]"></i>
      </button>
    </div>
    <div class="w-[33%]">
      <button
        v-for="(page, index) in TotalPage"
        :key="index"
        :style="{
          backgroundColor: index == this.zero ? '#fcaa3f' : '#f9c27b57',
        }"
        class="w-1/6 rounded-lg"
        @click="ClickPage(index)"
      >
        {{ index + 1 }}
      </button>
    </div>
    <div class="w-[33%]">
      <button @click="NextPage()">
        <i
          class="fa-solid fa-circle-arrow-right text-[20px] text-[#fcaa3f]"
        ></i>
      </button>
    </div>
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

<style>
.red {
  coloe: #f9c27b57;
}
</style>
