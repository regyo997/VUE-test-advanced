<template>
  <div
    class="container"
    style="border-width:1px;border-style:solid;border-color:#000000;padding:5px;"
  >
    <!--表單顯示與切換-->
    <div>Form {{currentPage.id}}</div>跳至
    <select @change="changePage">
      <option v-for="(thePage,index) of pages" :key="index" :value="index">{{thePage.id}}</option>
    </select>
    ,共 {{pages.length}} 張
    <!--Log按鈕-->
    <div>
      <button @click="logThisForm" class="btn btn-primary">Log this form</button>
      <button @click="logAllForm" class="btn btn-info">Log all form</button>
      <button
        @click="deleteThisForm"
        v-show="pages.length>1"
        class="btn btn-danger"
      >Delete this form</button>
    </div>

    <!--Tab換頁按鈕-->
    <div style="border-width:1px;border-style:solid;border-color:#CCCCCC;padding:5px;">
      <ul class="nav nav-tabs">
        <li v-for="(tab,index) of currentPage.records" :key="index">
          <button
            @click="changeTab"
            :value="tab.tab_name"
            class="btn btn-default"
          >{{showTabName(tab.tab_name)}}</button>
        </li>
      </ul>
      <br>

      <!--Tab主體-->
      <component :is="this.theTab" v-bind="tabInput" :modData="modData"></component>
    </div>

    <!--換頁按鈕-->
    <div class="row justify-content-center">
      <button @click="previousPage" class="btn btn-default">Previous</button>
      <button
        @click="changePage"
        v-for="(thePage,index) of pages"
        :key="index"
        :value="index"
        class="btn btn-default"
      >{{index+1}}</button>
      <button @click="nextPage" class="btn btn-default">Next</button>
    </div>
  </div>
</template>

<script>
import SelectPageComponent from "./SelectPageComponent.vue";
import PersonalInfoComponent from "./PersonalInfoComponent.vue";
import OrdersComponent from "./OrdersComponent.vue";
import OverviewComponent from "./OverviewComponent.vue";

export default {
  name: "PageComponent",
  components: {
    SelectPageComponent,
    personal_info: PersonalInfoComponent,
    orders: OrdersComponent,
    overview: OverviewComponent
  },
  data() {
    return {
      currentPageNum: 0,
      pages: [{ id: "test" }],
      theTab: "",
      theTabSequence: 0
    };
  },
  methods: {
    logThisForm() {
      console.log(this.pages[this.currentPageNum]);
    },
    logAllForm() {
      console.log(this.pages);
    },
    deleteThisForm() {
      if (this.currentPageNum < this.pages.length - 1) {
        this.pages.splice(this.currentPageNum, 1);
      } else {
        this.pages.splice(this.currentPageNum, 1);
        this.currentPageNum = this.pages.length - 1;
      }
    },
    previousPage() {
      if (this.currentPageNum > 0) this.currentPageNum--;
      this.theTab = this.currentPage.records[0].tab_name;
      this.theTabSequence = this.currentPage.records
        .map(x => x.tab_name)
        .indexOf(this.theTab);
    },
    nextPage() {
      if (this.currentPageNum < this.pages.length - 1) this.currentPageNum++;
      this.theTab = this.currentPage.records[0].tab_name;
      this.theTabSequence = this.currentPage.records
        .map(x => x.tab_name)
        .indexOf(this.theTab);
    },
    changePage(e) {
      this.currentPageNum = e.target.value;
      this.theTab = this.currentPage.records[0].tab_name;
      this.theTabSequence = this.currentPage.records
        .map(x => x.tab_name)
        .indexOf(this.theTab);
    },
    changeTab(e) {
      this.theTab = e.target.value;
      this.theTabSequence = this.currentPage.records
        .map(x => x.tab_name)
        .indexOf(this.theTab);
    },
    modData(e) {
      let value = e.target.value;
      const { type, name, checked } = e.target;
      if (this.theTab === "personal_info") {
        if (
          typeof this.pages[this.currentPageNum]["records"][
            this.theTabSequence
          ]["data"][name] === "number"
        )
          value = parseInt(value, 10);
        else if (type === "checkbox") value = checked;
        this.pages[this.currentPageNum]["records"][this.theTabSequence]["data"][
          name
        ] = value;
      } else if (this.theTab === "orders")
        if (type === "checkbox") {
          if (checked) {
            this.pages[this.currentPageNum]["records"][this.theTabSequence][
              "data"
            ]["banana_condiments"].push(value);
          } else {
            const index = this.pages[this.currentPageNum]["records"][
              this.theTabSequence
            ]["data"]["banana_condiments"].indexOf(value);
            this.pages[this.currentPageNum]["records"][this.theTabSequence][
              "data"
            ]["banana_condiments"].splice(index, 1);
          }
        } else {
          if (
            typeof this.pages[this.currentPageNum]["records"][
              this.theTabSequence
            ]["data"][name] === "number"
          )
            value = parseInt(value, 10);
          this.pages[this.currentPageNum]["records"][this.theTabSequence][
            "data"
          ][name] = value;
        }
    },
    showTabName(theTabName) {
      const arr = theTabName.split("_");
      let str = "";
      arr.forEach(
        x =>
          (str +=
            x.substring(0, 1).toUpperCase() + x.substring(1, x.length) + " ")
      );
      return str.trim();
    }
  },
  computed: {
    currentPage() {
      return this.pages[this.currentPageNum];
    },
    pageNumArray() {
      const arr = [];
      for (var i = 0; i < this.pages.length; i++) {
        arr.push(i + 1);
      }
      return arr;
    },
    pageIds() {
      return this.pages.map(page => page.id);
    },
    tabInput() {
      return this.currentPage;
    }
  },
  mounted: function() {
    this.pages = [
      {
        id: "A0000001",
        records: [
          {
            tab_name: "personal_info",
            tab_sequence: 0,
            data: {
              first_name: "Fishman",
              last_name: "ILike",
              gender: 1,
              address: "ABCDEFG",
              is_homeless: false,
              job: null,
              note: null
            }
          },
          {
            tab_name: "orders",
            tab_sequence: 1,
            data: {
              apple_count: 1,
              banana_condiments: ["chocolate", "chili", "garlic", "soy_sauce"]
            }
          },
          { tab_name: "overview", tab_sequence: 2 }
        ]
      },
      {
        id: "A0000002",
        records: [
          {
            tab_name: "orders",
            tab_sequence: 0,
            data: { apple_count: 15, banana_condiments: [] }
          },
          {
            tab_name: "personal_info",
            tab_sequence: 1,
            data: {
              first_name: null,
              last_name: "Somebody",
              gender: 2,
              address: "QWERTY",
              is_homeless: false,
              job: "secret_agent",
              note: "Hello, world"
            }
          },
          { tab_name: "overview", tab_sequence: 2 }
        ]
      },
      {
        id: "A0000003",
        records: [
          {
            tab_name: "personal_info",
            tab_sequence: 0,
            data: {
              first_name: "Painter",
              last_name: null,
              gender: 0,
              address: null,
              is_homeless: true,
              job: "agent_of_secret_agent",
              note: "Strange"
            }
          },
          {
            tab_name: "orders",
            tab_sequence: 1,
            data: { apple_count: 1, banana_condiments: ["herbal_cream"] }
          }
        ]
      },
      {
        id: "A0000004",
        records: [
          {
            tab_name: "orders",
            tab_sequence: 0,
            data: {
              apple_count: 100,
              banana_condiments: [
                "chocolate",
                "strawberry",
                "flax",
                "miso",
                "chili",
                "garlic",
                "soy_sauce",
                "thick_soy_sauce"
              ]
            }
          }
        ]
      },
      { id: "A0000005", records: [{ tab_name: "overview", tab_sequence: 0 }] }
    ];
    this.theTab = this.pages[0].records[0].tab_name;
    this.theTabSequence = this.pages[0].records[0].tab_sequence;
  }
};
</script>