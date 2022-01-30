<template>
  <div class="home">
    <div class="form-group">
      <div v-for="d in data" :key="d.id">
        <CustomField
        class="form-item"
          v-if="d.input_type == 'checkbox'"
          :type="d.input_type"
          :name="d.name"
          :description="d.description"
          :id="d.id"
          :validate="d.input_validator"
        />

        <CustomField
        class="form-item"
          v-else
          v-model="d.default_value"
          :type="d.input_type"
          :name="d.name"
          :placeholder="d.place_holder"
          :description="d.description"
          :id="d.id"
          :validate="d.input_validator"
          :class="d.css_style"
          @input="setName($event.target.value)"
        />
      </div>
      <p v-if="error" class="error">{{ error }}</p>
    <div class="buttons">
        <button @click="postFields()">Gönder</button>
      <button @click="updatePost(id)">Düzenlemeyi kaydet</button>
    </div>
    </div>
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
      error: "",
    };
  },
  beforeMount() {
    this.getFields();
    this.getPosts();
    this.default();
  },
  computed: {},
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
        if (element.default_value !== "") {
          postData["cf_" + element.id] = element.default_value;
          axios.post("http://localhost:3000/posts", postData);
          this.default();
        } else {
          this.error = "Lütfen tüm alanları doldurunuz";
        }
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
  },
  created: {},
  components: {
    CustomField,
  },
};
</script>


<style scoped lang="scss">
* {
  margin: 0%;
}
.home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.error {
  color: red;
}
button {
  margin: 5px;
  background-color: steelblue;
  border: none;
  border-radius: 6px;
  color: white;
  padding: 5px;
  cursor: pointer;
  max-height: 30px;
}
.buttons{
  margin-left: 50px;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.form-group {
  display: flex;
  flex-wrap: wrap;
  background-color: rgb(242, 247, 248);
width: 800px;
height: 500px;
align-items: center;
justify-content: center;
.form-item{
  width: 280px;
  max-width: 280px;
  margin-bottom: 10px;
  padding-left: 10px;
}
}

</style>