<template>
  <div class="ratings">
    <h2>신규 컨텐츠</h2>
    <ol v-if="executed">
      <li v-for="(item, index) in wpdata" :key="item.id" class="list">
        <router-link :to="'/quiz/'+item.id">
          <span class="count">{{index + 1}}.</span>
          <span class="clamp">{{item.title.rendered}}</span>
        </router-link>
      </li>
    </ol>
    <template v-else>
      <div class="list" v-for="n in parseInt(perPage)" :key="n">
        <loading-placeholder-line />
      </div>
    </template>
  </div>
</template>
    
<script>

import axios from 'axios'
import router from '../../routes'
import LoadingPlaceholderLine from '../loading-animation/LoadingPlaceholderLine.vue'

export default {
  router,
  name: 'SiteLogo',
  components: {
    LoadingPlaceholderLine
  },
  data(){
    return {
      executed: false,
      wpdata : [],
      perPage: 10
    }
  },
  mounted(){
    axios
      .get(`${window.projectURL}/wp-json/wp/v2/quiz?per_page=${this.perPage}&_fields=author,id,title,link`)
      .then(response => {
        this.wpdata =response.data;
      })
      .catch(error => {
        console.log(error);
      })
      .then(() => {
        this.executed = true
      });
  }
}
</script>

<style lang="scss">
  .ratings {
    padding: 30px;
    background-color: white;

    h2 {
      font-size: 1.25rem;
      font-weight: 500;
      color: #111;
      margin: 0 0 20px;
    }

    .list {
      border-bottom: 1px solid #e5e5e5;

      &:after {
        content: '';
        display: block;
        clear: both;
      }

      a {
        display: block;
        position: relative;
        padding: 10px 0 10px 25px;
        text-decoration: none;
        color: #777;

        &:hover, &:focus, &:active {
          color: #016afd;
        }

        .count {
          position: absolute;
          top: 10px;
          left: 0;
        }

        .clamp {
          overflow: hidden;
          display: -webkit-box;
          -webkit-box-orient: vertical;
          -webkit-line-clamp: 1;
          -o-text-overflow: ellipsis;
             text-overflow: ellipsis;
          max-height: 1;
        }
      }
    }
  }
</style>