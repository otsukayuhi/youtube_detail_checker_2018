<template lang="pug">
section.section
  h1.title YouTube Tags Checker
  input.input(type="text", v-model="id")
  button.btn(@click="getData") 調べる
  .detail
    .video-detail(:class="{'is-show': isShow}", v-if="!er")
      .video-img
        img(:src="infoImg")
      .video-title {{ infoTitle }}
      ul.video-tags(v-if="infoTags")
        li.video-tag(v-for="tag in infoTags") {{ tag }}
      .no-tags(v-if="!infoTags") タグはあません。
    .video-error(v-if="er") IDが間違っています。
</template>

<style lang="scss" scoped>
input,
button {
  appearance: none;
  vertical-align: top;
  border-radius: 5px;
}
.input {
  padding: 0 10px;
  width: 200px;
  height: 30px;
  border: 1px solid #000;
  font-size: 17px;
}
.btn {
  margin-left: 10px;
  width: 100px;
  height: 32px;
  font-size: 17px;
  cursor: pointer;
  border: 1px solid #000;
}
.detail {
  margin-top: 30px;
}
.video-title {
  font-size: 14px;
}
.video-detail {
  display: none;
  &.is-show {
    display: block;
  }
}
.video-tags {
  margin: 20px 0 0;
  padding: 0;
  display: flex;
  list-style: none;
}
.video-tag {
  margin: 3px;
  padding: 5px 20px;
  background-color: #111;
  font-size: 13px;
  border-radius: 5px;
  color: #fff;
}
</style>

<script>
import axios from 'axios'

export default {
  data () {
    return {
      id: '',
      er: false,
      isShow: false,
      infoTitle: null,
      infoTags: null,
      infoImg: null
    }
  },
  methods: {
    getData: function() {
      axios({
        methods: 'get',
        url: 'https://www.googleapis.com/youtube/v3/videos/',
        params: {
          part: 'snippet',
          id: this.id,
          key: 'AIzaSyButHaYE2QfxIhGmUrs9thFhDdKsgtytoQ'
        }
      })
      .then( response => {
        this.isShow = true;
        this.er = false;
        this.infoTitle = response.data.items[0].snippet.title;
        this.infoTags = response.data.items[0].snippet.tags;
        this.infoImg = response.data.items[0].snippet.thumbnails.medium.url;
      })
      .catch( error => {
        this.isShow = true;
        this.er = true;
      });
    }
  }
}
</script>
