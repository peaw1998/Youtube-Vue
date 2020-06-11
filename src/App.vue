<template>
  <div class="container">
    <div id="search">
      <h1 id="title">Youtube</h1>
      <div id="search-group">
        <input id="input" v-on:input="func" />
        <button id="button" v-on:click="youtube">Search</button>
      </div>

      <!-- <RedBox v-on:red="red" />/ -->
    </div>
    <div class="row">
      <div class="col-lg-8">
        <div class="embed-responsive embed-responsive-16by9">
          <iframe v-if="videoID" v-bind:src="ID" class="embed-responsive-item" />
          <iframe id="empty" v-else src />
        </div>
      </div>

      <div class="col-lg-4">
        <div id="exaple-border" v-if="checkArray" />
        <div id="example-video" v-bind:key="item" v-for="item in video" v-on:click="search(item)">
          <img
            v-bind:src="item.snippet.thumbnails.default.url"
            v-bind:width="item.snippet.thumbnails.default.width"
            v-bind:height="item.snippet.thumbnails.default.height"
          />
          <div id="name-video">{{ item.snippet.title }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import key from "./key";

export default {
  data: function() {
    return {
      input: " ",
      video: [],
      videoID: ""
    };
  },
  computed: {
    ID: function() {
      return "https://www.youtube.com/embed/" + this.videoID;
    }
  },
  components: {},
  mounted: function() {
    axios
      .get("https://www.googleapis.com/youtube/v3/search", {
        params: {
          key: key,
          type: "video",
          part: "snippet",
          q: ""
        }
      })
      .then(res => {
        this.video = res.data.items;
      });
  },
  methods: {
    func: function(event) {
      this.input = event.target.value;
    },
    youtube: function() {
      axios
        .get("https://www.googleapis.com/youtube/v3/search", {
          params: {
            key: key,
            type: "video",
            part: "snippet",
            q: this.input
          }
        })
        .then(res => {
          this.video = res.data.items;
        });
    },
    search: function(event) {
      this.videoID = event.id.videoId;
      console.log(this.videoID);
    },
    checkArray: function() {
      if (this.video.length === 0) return true;
      else return false;
    }
  }
};
</script>

<style scoped>
#title {
  display: inline-block;
}
#search-group {
  display: inline-block;
}
#input {
  width: 75%;
}
#button {
  width: 25%;
}
#search {
  margin-bottom: 15px;
}
#example-video {
  display: flex;
  justify-content: space-between;
  border: 1px solid black;
  padding: 10px;
  cursor: pointer;
}
#name-video {
  width: 50%;
}
#empty {
  border: 1px solid black;
}

/* Mobile L -- */
@media only screen and (max-width: 427px) {
  #title {
    width: 100%;
  }
  #search-group {
    width: 100%;
  }
}

/* Mobile L ++ */
@media only screen and (min-width: 427px) {
  #title {
    width: 50%;
  }
  #search-group {
    width: 50%;
  }
}
</style>
