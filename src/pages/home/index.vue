<style lang="less">
  @import '../../assets/font/iconfont.css';
  .post-content-container {
    margin-bottom: 80px;
  }
  .disabled {
    display: none;
  }
  .notice {
    &-wrap {
      z-index: 100;
      position: fixed;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, .5);
    }
    & {
      position: absolute;
      top: 50%;
      left: 50%;
      margin-left: -300px;
      margin-top: -380px;
      padding: 0 30px;
      width: 540px;
      border-radius: 5px;
      background-image: url('../../assets/images/notice.png');
      background-size: 100% 100%;
      background-repeat: no-repeat;
      p {
        margin-top: 280px;
        margin-bottom: 50px;
        font-size: 26px;
        color: #777;
      }
      .btn {
        margin-bottom: 54px;
      }
    }
  }
  .slider-wrap {
    width: 100%;
    height: 300px;
  }
  .slider-list {
    width:  100%;
    height: 100%;
  }
  .slider-list-image {
    width:  100%;
    height: 100%;
  }
  .post-container {
    margin-top: 45px;
    margin-bottom: 50px;
    overflow: hidden;
    padding-bottom: 10px;
    li {
      position: relative;
      float: left;
      width: 50%;
      height: 64px;
      text-align: center;
      font-size: 28px;
      background-color: #fff;
      color: #76c5fd;
      line-height: 64px;
    }
    li:nth-child(1) {
      border-radius: 5px 0 0 5px;
    }
    li:nth-child(2) {
      border-radius: 0 5px 5px 0;
    }
    li.active {
      background-color: #76c5fd;
      color: #fff;
      &::after {
        content: "";
        display: block;
        position: absolute;
        top: 100%;
        left: 50%;
        margin-left: -16px;
        border-style: solid;
        border-width: 6px 8px 0 8px;
        border-color: #76c5fd transparent transparent transparent;
      }
    }
  }
.post-categorys-container {
  display: flex;
  flex-wrap: wrap;
  overflow: hidden;
  justify-content: space-between;
  overflow: hidden;
  margin-bottom: 60px;
  width: 100%;
  .category-item {
    position: relative;
    margin-top: 20px;
    margin-left: 55px;
    margin-right: 55px;
    width: 115px;
    height: 158px;
    background-size: 100% 100%;
    div {
      display: none;
      position: absolute;
      top: 2px;
      left: 2px;
      width: 101px;
      height: 97px;
      border-radius: 50%;
      border: 3px solid #73dce8;
    }
  }
  .category-router-active {
    div {
      display: block;
    }
  }
  .category-sharing {
    background-image: url('../../assets/images/category_sharing.png');
  }
  .category-life {
    background-image: url('../../assets/images/category_live.png')
  }
  .category-learning {
    background-image: url('../../assets/images/category_leaning.png')
  }
  .category-technology {
    background-image: url('../../assets/images/category_tecochnology.png')
  }
  .category-employment {
    background-image: url('../../assets/images/category_employment.png')
  }
  .category-others {
    background-image: url('../../assets/images/category_others.png')
  }
}
</style>

<template>
  <div>
    <!-- 公告 -->
    <div style="display: none" v-show="noticeShow"  class="notice-wrap">
      <div class="notice">
        <p>{{notice.content}}</p>
        <btn v-on:click.native="noticeConfirm">确定</btn>
      </div>
    </div>

    <!-- cover -->
    <div class="slider-wrap">
      <a class="slider-list" href="##">
        <img class="slider-list-image" :src="info.index_url">
      </a>
    </div>

    <container class="post-content-container">
      <ul class="post-categorys-container">
        <router-link
          tag="li"
          class="category-item category-sharing"
          :to="`/home/${type}/${sharing_id}`"
          activeClass="category-router-active"
        >
          <div></div>
        </router-link>
        <router-link
          tag="li"
          class="category-item category-life"
          :to="`/home/${type}/${life_id}`"
          activeClass="category-router-active"
        >
          <div></div>
        </router-link>
        <router-link
          tag="li"
          class="category-item category-learning"
          :to="`/home/${type}/${learing_id}`"
          activeClass="category-router-active"
        >
          <div></div>
        </router-link>
        <router-link
          tag="li"
          class="category-item category-technology"
          :to="`/home/${type}/${technology_id}`"
          activeClass="category-router-active"
        >
          <div></div>
        </router-link>
        <router-link
          tag="li"
          class="category-item category-employment"
          :to="`/home/${type}/${employment_id}`"
          activeClass="category-router-active"
        >
          <div></div>
        </router-link>
        <router-link
          tag="li"
          class="category-item category-others"
          :to="`/home/${type}/${others_id}`"
          activeClass="category-router-active"
        >
          <div></div>
        </router-link>
      </ul>
      <ul class="post-container">
        <router-link tag="li" activeClass="active" :to="`/home/new/${category_id}`" replace>最新</router-link>
        <router-link tag="li" activeClass="active" :to="`/home/hot/${category_id}`" replace>最热</router-link>
      </ul>
      <keep-alive>
        <router-view :categoriesMap="categoriesMap"  style="margin-bottom: 60px;"></router-view>
      </keep-alive>
    </container>
  </div>
</template>


<script>
  import container from '../../components/container'
  import btn from '../../components/btn'
  import util from '../../util'

  const categoriesMap = {
    '学习': 'learing',
    '生活': 'life',
    '技术': 'technology',
    '就业': 'employment',
    '分享': 'sharing'
  }

  export default  {
    name: 'home',
    components: {
      container,
      btn
    },
    data() {
      return {
        info: {},
        notice: {},
        preNoticeId: ~~window.localStorage.getItem('noticeId'),
        noticeShow: false,
        type: 'new',
        category_id: 0,
        learing_id: -2, // -2是没有没有该分类 -1是全部分类
        life_id: -2,
        others_id: 0, // 其他类别 id 为0
        technology_id: -2,
        employment_id: -2,
        sharing_id: -2,
        categoriesMap: {}
      }
    },
    methods: {
      noticeConfirm() {
        window.localStorage.setItem('noticeId', this.notice.id)
        this.preNoticeId = this.notice.id
        this.noticeShow = false
      }
    },
    created() {
      this.type = this.$route.params['type']
      this.category_id = this.$route.params['id']

      this.$http.get('/notice')
        .then((res) => {
          this.notice = res.body
          if(this.notice.content !== void 0 && this.notice.id !== this.preNoticeId) {
            this.noticeShow = true
          }
        })
        .catch(console.error)

      this.$http.get('/info')
        .then((res) => {
          this.info = res.body
          document.title = this.info.name || '问答平台'
          util.ls.set('info', this.info)
          this.info.categories.forEach((item) => {
            if(categoriesMap[item.name]) {
              let category_id = categoriesMap[item.name] + '_id'
              this[category_id] = item.id
              this.categoriesMap[item.id] = item.name
            }
          })
        })
        .catch(console.error)
    },
    beforeRouteUpdate(to, from, next) {
      this.id = ~~to.params['id']
      this.type = to.params['type']

      // id 是-2表明没有该分类
      this.category_id = this.id
      next(true)
    },
  }
</script>
