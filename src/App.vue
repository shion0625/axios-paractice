<template>
  <div id="app">
    <h3>掲示板に投稿する</h3>
    <label for="name">ニックネーム：</label>
    <input
      id="name"
      type="text"
      v-model="name"
    >
    <br><br>
    <label for="comment">コメント：</label>
    <textarea
      id="comment"
      v-model="comment"
    ></textarea>
    <br><br>
    <button @click="createComment">コメントをサーバーに送る</button>
    <h2>掲示板</h2>
    <div
      v-for="post in posts"
      :key="post.name"
    >
      <br>
      <div>名前：{{post.fields.name.stringValue}}</div>
      <div>コメント：{{post.fields.comment.stringValue}}</div>
    </div>
  </div>
</template>

<script>
import axios from './axios-auth';

export default {
  data() {
    return {
      name: '',
      comment: '',
      posts: []
    };
  },
  created() {
    axios.get('/comments').then(response => {
      this.posts = response.data.documents;
    });
  },
  methods: {
    createComment() {
      axios.post('/comments', {
        fields: {
          name: {
            stringValue: this.name
          },
          comment: {
            stringValue: this.comment
          }
        }
      });
      this.name = '';
      this.comment = '';
    }
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
