<template>
  <div>
    <!--誰-->
    以下是{{theName}}的訂單
    <div class="container">
      <div class="row">
        <div class="col">
          <div v-if="orders">
            <li style="list-style-type:disc;">{{orders.data.apple_count}}顆蘋果</li>
            <li style="list-style-type:disc;" v-show="!isBananaNull">香蕉配料</li>
            <ul>
              <li
                v-for="item of orders.data.banana_condiments"
                :key="item"
                style="list-style-type:circle"
              >{{ingredients[item]}}</li>
            </ul>
          </div>
          <div v-else>"No data found"</div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      ingredients: {
        chocolate: "巧克力醬",
        strawberry: "草莓醬",
        flax: "胡麻醬",
        miso: "味噌",
        chili: "辣椒",
        garlic: "大蒜",
        soy_sauce: "醬油",
        thick_soy_sauce: "醬油膏",
        herbal_cream: "百香膏"
      }
    };
  },
  props: {
    id: {
      type: String
    },
    records: {
      type: Array
    }
  },
  computed: {
    personalInfo() {
      if (this.records.map(x => x.tab_name).indexOf("personal_info") !== -1)
        return this.records[
          this.records.map(x => x.tab_name).indexOf("personal_info")
        ];
      else return { data: { first_name: "unknown", last_name: null } };
    },
    orders() {
      if (this.records.map(x => x.tab_name).indexOf("orders") !== -1)
        return this.records[
          this.records.map(x => x.tab_name).indexOf("orders")
        ];
      else return false;
    },
    theName() {
      const firstName =
        this.personalInfo.data.first_name === null
          ? ""
          : this.personalInfo.data.first_name;
      const lastName =
        this.personalInfo.data.last_name === null
          ? ""
          : this.personalInfo.data.last_name + " ";
      return lastName + firstName === "" ? "unknown" : lastName + firstName;
    },
    isBananaNull() {
      return this.orders.data.banana_condiments === null;
    }
  }
};
</script>
