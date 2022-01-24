<template>
  <div><div class="form-group">
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
    <button @click="postFields()">Gönder</button>
    <button @click="updatePost(id)">Düzenlemeyi kaydet</button></div>
    <li v-for="post in posts" :key="post.id">
      {{ post.id }}
      <button class="btn btn-primary" @click="editPosts(post)">Düzenle</button>
    </li>
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
      post: "123123",
      updateId: null,
    };
  },
  beforeMount() {
    this.getFields();
    this.getPosts();
    this.default();
  },
  methods: {
    getFields() {
      axios
        .get("http://localhost:3000/fields")
        .then((response) => (this.data = response.data));
    },
    getPosts() {
      axios
        .get("http://localhost:3000/posts")
        .then((response) => (this.posts = response.data));
    },
    postFields() {
      var postData = {};
      this.data.forEach((element) => {
        postData["cf_" + element.id] = element.default_value;
      });
      axios.post("http://localhost:3000/posts", postData);
      this.default();
    },
    default() {
      this.data.forEach((element) => {
        element.default_value = null;
      });
    },
    editPosts(data) {
      var xyz = Object.values(data);
      this.data.forEach((element, index) => {
        element.default_value = xyz[index];
        this.updateId = xyz[xyz.length - 1];
      });
    },
    updatePost() {
      var putData = {};
      this.data.forEach((element) => {
        putData["cf_" + element.id] = element.default_value;
      });
      axios.put("http://localhost:3000/posts/" + this.updateId, putData);
      this.default();
    },
  },
  created: {},
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

.form-group{
  display: grid;
  
  justify-content: space-around;
}
</style>