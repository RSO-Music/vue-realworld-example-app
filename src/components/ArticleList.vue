<template>
  <div>
    <!--    <div v-if="isLoading" class="article-preview">Loading articles...</div>-->
    <div>
      <!--      <div v-if="this.songs.length === 0" class="article-preview">-->
      <!--        No songs are here.. . yet.-->
      <!--      </div>-->
      <RwvArticlePreview
        v-for="song in this.songs"
        :article="song"
        :key="song.songId"
      />

      <!--      <RwvArticlePreview-->
      <!--        v-for="(article) in articles"-->
      <!--        :article="article"-->
      <!--        :key="article.title"-->
      <!--      />-->
      <!--      <VPagination :pages="pages" :currentPage.sync="currentPage" />-->
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import RwvArticlePreview from "./VArticlePreview";
import VPagination from "./VPagination";
import { FETCH_ARTICLES } from "../store/actions.type";

export default {
  name: "RwvArticleList",
  components: {
    RwvArticlePreview
    // ,
    // VPagination
  },
  // props: {
  // type: {
  //   type: String,
  //   required: false,
  //   default: "all"
  // },
  // author: {
  //   type: String,
  //   required: false
  // }
  // ,
  // tag: {
  //   type: String,
  //   required: false
  // }
  // ,
  // favorited: {
  //   type: String,
  //   required: false
  // }
  // ,
  // itemsPerPage: {
  //   type: Number,
  //   required: false,
  //   default: 10
  // }
  // },
  data() {
    return {
      currentPage: 1
    };
  },
  // computed: {
  //   listConfig() {
  //     const { type } = this;
  //     return {
  //       type
  //     };
  //   },
  //   pages() {
  //     if (this.isLoading || this.articlesCount <= this.itemsPerPage) {
  //       return [];
  //     }
  //     return [
  //       ...Array(Math.ceil(this.articlesCount / this.itemsPerPage)).keys()
  //     ].map(e => e + 1);
  //   },
  //   ...mapGetters(["articlesCount", "isLoading", "articles"])
  // }
  // ,
  // watch: {
  // currentPage(newValue) {
  //   this.listConfig.filters.offset = (newValue - 1) * this.itemsPerPage;
  //   this.fetchArticles();
  // }
  // ,
  // type() {
  //   this.resetPagination();
  //   this.fetchArticles();
  // },
  // author() {
  //   this.resetPagination();
  //   this.fetchArticles();
  // },
  // tag() {
  //   this.resetPagination();
  //   this.fetchArticles();
  // },
  // favorited() {
  //   this.resetPagination();
  //   this.fetchArticles();
  // }
  // }
  // ,
  updated() {
    this.fetchArticles();
  },
  loaded() {
    this.fetchArticles();
  },
  mounted() {
    this.fetchArticles();
  },
  created() {
    this.fetchArticles();
  },
  methods: {
    fetchArticles() {
      //this.$store.dispatch(FETCH_ARTICLES, this.listConfig);
      console.log(this.listConfig);
      fetch("http://34.89.111.13/v1/songs")
        .then(Response => Response.json())
        .then(json => {
          this.songs = json;
          //this.listConfig = this.songs;
          console.log(this.songs);
        });
    },
    resetPagination() {
      // this.listConfig.offset = 0;
      this.currentPage = 1;
    }
  }
};
</script>
