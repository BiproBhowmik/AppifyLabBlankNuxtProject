<template>
  <div class="container">
    <div>
      <button class="dropbtn" @click="showAddPostModal()">Add Post</button>
      <div class="list_div" v-for="(item, i) in error" :key="i">
        <h3 class="d-inline-block pl-2">{{ item }}</h3>
      </div>
      <h1>blog page</h1>

      <div v-if="showAddModal" class="modal-body">
        <form @submit.prevent="addPost(postObject)" method="post">
          <input
            class="form-control"
            v-model="postObject.title"
            placeholder="Post title"
          />
          <textarea
            v-model="postObject.discription"
            placeholder="Post discription"
          ></textarea>
          <input
            class="form-control"
            v-model="postObject.author"
            placeholder="Author name"
          />
          <button class="dropbtn">
            <!-- <button class="dropbtn" @click.stop="addPost(postObject)"> -->
            Add Post
          </button>
        </form>
        <button
          class="dropbtn"
          @click="
            (showAddModal = false), (showEditModal = false), error.splice(0)
          "
        >
          Cancle
        </button>
      </div>

      <div v-if="showEditModal" class="modal-body">
        <form @submit.prevent="updatePost(editPostObj)" method="post">
          <input
            class="form-control"
            v-model="editPostObj.title"
            placeholder="Post title"
          />
          <textarea
            v-model="editPostObj.discription"
            placeholder="Post discription"
          ></textarea>
          <input
            class="form-control"
            v-model="editPostObj.author"
            placeholder="Author name"
          />
          <button class="dropbtn">
            Edit Post
          </button>
        </form>
        <button
          class="dropbtn"
          @click="
            (showAddModal = false), (showEditModal = false), error.splice(0)
          "
        >
          Cancle
        </button>
      </div>

      <div class="list_div" v-for="(item, i) in posts" :key="i">
        <h2 class="d-inline-block">title: {{ item.title }}</h2>
        <h3 class="d-inline-block pl-2">disription: {{ item.discription }}</h3>
        <h3 class="d-inline-block pl-2">author: {{ item.author }}</h3>

        <div class="dropdown float-right">
          <div class="dropdown-content">
            <a @click="editPost(item, i)">Edit</a>
            <a @click.stop="deletePost(item, i)">Delete</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [],
      showAddModal: false,
      showEditModal: false,
      postObject: {
        title: "",
        discription: "",
        author: ""
      },
      editPostObj: "",
      error: []
    };
  },
  methods: {
    async addPost(postObj) {
      this.error.splice(0);
      if (postObj.title == "") {
        this.error.push("Title is required");
        return;
      }

      if (postObj.author == "") {
        this.error.push("Author Name is required");
        return;
      }

      const res = await this.callApi(
        "post",
        "http://localhost:3333/storePost",
        postObj
      );

      if (res.status == 200) {
        this.posts.push(res.data);
        this.showAddModal = false;
      }
    },

    async updatePost(postObj) {
      this.error.splice(0);
      if (postObj.title == "") {
        this.error.push("Title is required");
        return;
      }

      if (postObj.author == "") {
        this.error.push("Author Name is required");
        return;
      }
      const res = await this.callApi(
        "post",
        "http://localhost:3333/updatePost",
        postObj
      );

      if (res.status == 200) {
        // this.posts.push(res.data)
        this.showAddModal = false;
        this.showEditModal = false;
      }
    },

    async deletePost(postObj, index) {
      // console.log(categoryObj)
      const res = await this.callApi(
        "post",
        "http://localhost:3333/destroyPost",
        postObj
      );
      if (res.status == 200) {
        this.posts.splice(index, 1);
        //console.log(this.posts)
      }
    },

    async clearFilds() {
      // console.log(categoryObj)
      this.postObject.title = "";
      this.postObject.discription = "";
      this.postObject.author = "";
    },

    async showAddPostModal() {
      this.showEditModal = false;
      this.showAddModal = true;
      this.postObject.title = "";
      this.postObject.discription = "";
      this.postObject.author = "";
      // console.log(categoryObj)
    },

    async editPost(postObj, index) {
      this.showEditModal = true;
      this.showAddModal = false;
      this.editPostObj = postObj;
      //    this.postObject = postObj
    }
  },
  async created() {
    const res = await this.callApi("get", "http://localhost:3333/showAllPost");
    if (res.status == 200) {
      this.posts = res.data;
      //console.log(this.posts)
    }
  },
  components: {}
};
</script>

<style scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  padding: 50px;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

/* Modal Start*/
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}
/* Modal End*/
</style>
