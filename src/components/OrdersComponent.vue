<template>
  <div>
    <div class="container">
      <div class="row">
        <div class="col" style="width: 100%;padding-left:2px;">
          <!--Apple-->
          Apple
          <button
            name="apple_count"
            value="-5"
            @click="changeAppleCount"
            class="btn btn-sm btn-default"
          >-5</button>
          <button
            name="apple_count"
            value="-1"
            @click="changeAppleCount"
            class="btn btn-sm btn-default"
          >-1</button>
          
          <input
            type="text"
            name="apple_count"
            :value="apple_count"
            @change="modData"
            onkeyup="value=value.replace(/^|[^\d]+/g,'')"
            style="width: 40%;padding-left:2px;"
          >
          <button
            name="apple_count"
            value="+1"
            @click="changeAppleCount"
            class="btn btn-sm btn-default"
          >+1</button>
          <button
            name="apple_count"
            value="+5"
            @click="changeAppleCount"
            class="btn btn-sm btn-default"
          >+5</button>
          <br>
          <br>
          <!--Banana-->
          Banana
          <br>配料
          <br>
          <span v-for="(ingredient,index) of ingredients" :key="index">
            <input
              @change="modData"
              type="checkbox"
              name="banana_condiments"
              :value="ingredient"
              v-model="banana_condimentsGetter"
            >
            {{index}}
          </span>
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
        巧克力醬: "chocolate",
        草莓醬: "strawberry",
        胡麻醬: "flax",
        味噌: "miso",
        辣椒: "chili",
        大蒜: "garlic",
        醬油: "soy_sauce",
        醬油膏: "thick_soy_sauce",
        百香膏: "herbal_cream"
      }
    };
  },
  props: {
    modData: {
      type: Function
    },
    id: {
      type: String
    },
    records: {
      type: Array
    }
  },
  methods: {
    changeAppleCount(e) {
      let value = this.apple_count + parseInt(e.target.value, 10);
      const { type, name } = e.target;
      value = value > 0 ? value : 0;
      const e2 = { target: { name: name, type: type, value: value } };
      this.modData(e2);
    }
  },
  computed: {
    orders() {
      return this.records[this.records.map(x => x.tab_name).indexOf("orders")];
    },
    apple_count() {
      return this.orders.data.apple_count;
    },
    banana_condiments() {
      return this.orders.data.banana_condiments;
    },
    banana_condimentsGetter: {
      get: function() {
        return this.banana_condiments;
      },
      set: function() {}
    }
  }
};
</script>

