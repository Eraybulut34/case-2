<template>
  <div>
    <div v-for="d in data" :key="d.id">
      <CustomField
        v-model="d.id"
        :type="d.input_type"
        :name="d.name"
        :placeholder="d.place_holder"
        :description="d.description"
        :id="d.id"
      />
    </div>
    <button @click="postFields()">Gönder</button>
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
      post: 123123,
    };
  },
  beforeMount() {
    this.getFields();
    this.getPosts();
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
    editPosts(data) {
      for(let d in data.length()){
      d.default_value = this.posts[data];console.log(d);}

    },
    created: {},
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