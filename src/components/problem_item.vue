<style lang="less">
.problem-list {
  margin-bottom: 25px;
  padding: 25px 15px;
  width: 100%;
  overflow: hidden;
  border-radius: 5px;
  box-sizing: border-box;

  &-user-info {
    line-height: 40px;
    margin-bottom: 25px;
    .isFavorite {
      &::after {
        content: "\E7a3";
      }
    }
  }
  &-header {
    display: inline-block;
    width: 40px;
    height: 40px;
    border: 1px solid #76c5fd;
    border-radius: 50%;
    vertical-align:middle;
  }
  &-username {
    margin-left: 10px;
    font-weight: bolder;
    font-size: 24px;
    color: #8e9fa1;
  }
  &-collect {
    float: right;
    width: 50px;
    height: 50px;
    font-size: 50px;
    color: #76c5fd;
    &::after {
      content: "\E682"
    }
  }
  &-title {
    font-size: 28px;
    line-height: 30px;
    .type {
      font-size: 24px;
    }
  }
  &-intro {
    display: -webkit-box;
    margin-bottom: 36px;
    width: 100%;
    overflow: hidden;
    line-height: 36px;
    font-size: 24px;
    color: #3e4546;
    /*3行省略*/
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
  }
  &-pics {
    overflow: scroll;
    margin-bottom: 50px;
    img {
      margin-right: 30px;
      width: 190px;
      height: 190px;
      border: 1px solid #76c5fd;
      border-radius: 5px;
    }
    img:last-child {
      margin-right: 0;
    }
  }

}
.problem-list-time-comments {
  overflow: auto;
  font-size: 24px;
  color: #999;
  .problem-list-time {
    float: left;
  }
  .problem-list-comments {
    float: right;
  }
}

</style>

<template>
  <bg-container class="problem-list">
    <router-link :to="`/user/${problem.user_id}`">
      <p class="problem-list-user-info">
        <img class="problem-list-header" :src="problem.user && problem.user.headimgurl">
        <span class="problem-list-username">{{problem.user && problem.user.nickname}}</span>
        <i
          v-show="problem.self !== false"
          class="iconfont problem-list-collect"
          :class="{isFavorite: problem.isFavorite}"
          v-on:click.stop.prevent="favorite(problem.index, problem.id)"
          ></i>
      </p>
    </router-link>
    <h3 class="problem-list-title">
      {{problem.title}}
      <type>{{(problem.category && problem.category.name)||'其他'}}</type>
    </h3>
    <p class="problem-list-intro">
      {{problem.content}}
    </p>
    <div v-if="problem.image_url && problem.image_url[0]" class="problem-list-pics">
      <img v-for="url in problem.image_url" :key="url" :src="url">
    </div>
    <div class="problem-list-time-comments">
      <span class="problem-list-time">{{problem.updated_at}}</span>
      <span class="problem-list-comments" href="##">{{problem.reply_count}}条评论</span>
    </div>
  </bg-container>
</template>

<script>
  import bgContainer from './bg-container'
  import type from './type'

  export default {
    props: {
      problem: {
        type: Object,
        default() {
          return {}
        }
      }
    },
    name: 'problem-item',
    components: {
      bgContainer,
      type
    },
    methods: {
      favorite() {
        this.$emit('favorite', this.problem.index, this.problem.id)
      }
    },
    created() {
    }
  }
</script>
