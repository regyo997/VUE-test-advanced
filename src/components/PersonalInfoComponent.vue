<template>
  <div>
    <!--Name-->
    <div class="container">
      <div class="row">
        <span class="col" style="width: 100%;padding-left:2px;">First name</span>
        <span class="col" style="width: 100%;padding-left:2px;">Last name</span>
      </div>
      <div class="row">
        <input
          type="text"
          @change="modData"
          name="first_name"
          :value="first_name"
          class="col"
          style="width: 100%;padding-left:2px;"
        >
        <input
          type="text"
          @change="modData"
          name="last_name"
          :value="last_name"
          class="col"
          style="width: 100%;padding-left:2px;"
        >
      </div>
      <br>

      <!--Gender-->
      <div class="row">
        <div class="col" style="width: 100%;padding-left:2px;">Gender</div>
        <div class="col-10">
          <input @change="modData" type="radio" name="gender" v-model="genderProcess" value="0">Secret
          <br>
          <input @change="modData" type="radio" name="gender" v-model="genderProcess" value="1">Male
          <br>
          <input @change="modData" type="radio" name="gender" v-model="genderProcess" value="2">Female
        </div>
      </div>
      <br>
      <!--Address-->
      <div class="row" style="width: 100%;padding-left:2px;">
        Address
        <input
          type="text"
          @change="modData"
          name="address"
          :value="address"
          :disabled="is_homeless"
          class="container"
          style="width: 100%;padding-left:2px;"
        >
        <br>
        <div>
          <input type="checkbox" @change="modData2" name="is_homeless" v-model="is_homelessProcess">此客戶居無定所
        </div>
      </div>
      <br>
      <!--Job-->
      <div class="row" style="width: 100%;padding-left:2px;">
        Job
        <select @change="modData" name="job" v-model="jobProcess">
          <option value="null">保密</option>
          <option value="agent">調查員</option>
          <option value="secret_agent">秘密調查員</option>
          <option value="agent_of_secret_agent">秘密調查員的調查員</option>
        </select>
      </div>
      <br>
      <!--Note-->
      <div class="row" style="width: 100%;padding-left:2px;">Note
        <br>
        {{countChars}}/2000 characters
      </div>
      <div class="row" style="width: 100%;padding-left:2px;">
        <textarea
          rows="5"
          @input="modData"
          type="text"
          name="note"
          :value="note"
          maxlength="2000"
          class="row-1"
          style="width: 100%;padding-left:2px;"
        ></textarea>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "PersonalInfoComponent",
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
  computed: {
    personalInfo() {
      return this.records[
        this.records.map(x => x.tab_name).indexOf("personal_info")
      ];
    },
    first_name() {
      return this.personalInfo.data.first_name;
    },
    last_name() {
      return this.personalInfo.data.last_name;
    },
    gender() {
      return this.personalInfo.data.gender;
    },
    address() {
      return this.personalInfo.data.address;
    },
    is_homeless() {
      return this.personalInfo.data.is_homeless;
    },
    job() {
      return this.personalInfo.data.job;
    },
    note() {
      return this.personalInfo.data.note;
    },
    genderProcess: {
      get: function() {
        return this.gender;
      },
      set: function() {}
    },
    jobProcess: {
      get: function() {
        return this.job;
      },
      set: function() {}
    },
    is_homelessProcess: {
      get: function() {
        return this.is_homeless;
      },
      set: function() {}
    },
    countChars() {
      if (this.note == null) return 0;
      else return this.note.length;
    }
  },
  methods: {
    modData2(e) {
      const e2 = { target: { name: "address", value: "", type: "text" } };
      this.modData(e2);
      this.modData(e);
    }
  }
};
</script>
