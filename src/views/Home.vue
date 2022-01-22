<template>
  <div>
    <div v-for="d in data" :key="d.id">
      <CustomField
        v-model="d.default_value"
        :type="d.input_type"
        :name="d.name"
        :placeholder="d.place_holder"
        :description="d.description"
        :id="d.id"
      />
    </div>
    <button @click="postFields()">sadasdas</button>
  </div>
</template>

<script>
import axios from "axios";
import CustomField from "@/components/CustomField.vue";

export default {
  name: "Home",
  data() {
    return {
      data: null,
      posts: null,
      xxx: "test",
    };
  },
  beforeMount() {
    this.getFields();
  },
  methods: {
    getFields() {
      axios
        .get("http://localhost:3000/fields")
        .then((response) => (this.data = response.data));
      Array.from();
    },
    postFields() {
      var postData = {};
      this.data.forEach((element) => {
        postData["cf_" + element.id] = element.default_value;
      });
      axios.post("http://localhost:3000/posts", postData);
    },
  },
  components: {
    CustomField,
  },
};
</script>


<style scoped>
* {
  margin: 0%;
}
.form-action {
  display: block;
  justify-content: space-around;
}
</style>