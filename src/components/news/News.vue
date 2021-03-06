<template>
  <transition name="app-fade-left">
    <app-heart-loader v-if="this.isLoading"></app-heart-loader>
    <div v-else>
      <transition name="app-fade-left">
        <app-theme-page-title v-show="isLoadedData">
          <p slot="middleTitle">АКЦИИ И НОВОСТИ</p>
          <p slot="bottomTitle">Самые привлекательные предложения и жгучие новости</p>
        </app-theme-page-title>
      </transition>
      <transition name="app-fade-right">
        <el-row v-show="isLoadedData" id="filter_block" type="flex">
          <el-col align="right">
            <el-radio-group @change="changeType" v-model="type" id="news_types">
              <el-radio label="all">Все</el-radio>
              <el-radio label="sale">Акции</el-radio>
              <el-radio label="article">Новости</el-radio>
            </el-radio-group>
          </el-col>
        </el-row>
      </transition>
      <!--NEWS 1-->
      <div v-if="news" id="news_wrapper">
        <div v-for="oneNews in news"
             :key="oneNews.id"
             v-if="type === 'all' ? true : ( oneNews.type === type )"
             class="news_block">
          <transition name="app-fade-left">
            <el-row v-show="isLoadedData" class="news_tag_row" type="flex" justify="left">
              <el-col class="news_tag" align="left">
                {{ oneNews.type === 'sale' ? 'А к ц и и' : 'Н о в о с т и' }}
                <div class="red_line"></div>
              </el-col>
            </el-row>
          </transition>
          <el-row type="flex" justify="center" style="flex-wrap: wrap">
            <transition name="app-fade-left">
              <el-col v-show="isLoadedData" :xs="20" :sm="9" :md="9" :lg="9" :xl="9">
                <v-card id="news_img" class="white" height="320px">
                  <v-card-media
                    v-if="oneNews.img_0"
                    :src="oneNews.img_0.original"
                    height="320px">
                  </v-card-media>
                </v-card>
              </el-col>
            </transition>
            <transition name="app-fade-right">
              <el-col v-show="isLoadedData" class="news_text_block" :xs="20" :sm="9" :md="9" :lg="9" :xl="9" align="left">
                <p class="news_title">
                  {{ oneNews.title }}
                </p>
                <div class="news_red_line"></div>
                <p class="news_public_date">
                  Опубликовано {{ oneNews.creationDate | newsDate }}
                </p>
                <p v-if="oneNews.description"
                   v-html="oneNews.description.slice(0, 160) + '...'"
                   class="new_descr_snippet"></p>
                <router-link :to="'/news/' + oneNews.id">
                  <app-theme-btn>
                    Узнать больше
                  </app-theme-btn>
                </router-link>
              </el-col>
            </transition>
          </el-row>
        </div>
      </div>
      <el-row v-if="totalNewsCount > 5" type="flex" justify="center" class="mt-2">
        <el-pagination
          id="news_pagination"
          @current-change="changeCurPage"
          @size-change="changePageSize"
          layout="prev, pager, next"
          :current-page.sync="curPage"
          :page-size="pageSize"
          :total="totalNewsCount">
        </el-pagination>
      </el-row>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'News',
  data () {
    return {
      type: 'sale',
      isLoadedData: true,
      curPage: 1,
      pageSize: 5
    }
  },
  methods: {
    loadNews () {
      this.type = this.$store.getters.loadedNewsType
      this.$store.dispatch('loadNews', {type: this.$store.getters.loadedNewsType})
    },
    changeCurPage (curPage) {
      this.curPage = curPage
      window.smoothscroll() // npm back-to-top method!
    },
    changePageSize (size) {
      this.pageSize = size
    },
    changeType (type) {
      this.curPage = 1
      this.$store.dispatch('loadNews', {type: type})
    }
  },
  computed: {
    news () {
      if (this.$store.getters.news) {
        return Object.values(this.$store.getters.news)
          .slice((this.curPage - 1) * this.pageSize, this.curPage * this.pageSize)
      } else {
        return []
      }
    },
    totalNewsCount () {
      return this.$store.getters.news ? Object.keys(this.$store.getters.news).length : 0
    }
  },
  created () {
    this.loadNews()
  },
  watch: {
    isLoading () {
      this.$nextTick(function () {
        this.isLoadedData = !this.isLoadedData
      })
    }
  }
}
</script>

<style scoped lang="scss">
  #news_wrapper {
    margin-bottom: 60px;
  }

  #filter_block {
    color: white;
    font-size: 10px;
    font-weight: 600;
    margin-right: 80px;
  }

  .news_tag_row {
    margin-top: 14px;
    margin-bottom: 5px;
  }

  .news_tag {
    color: white;
    font-size: 17px;
    font-weight: 600;
    margin-left: 22px;
  }

  .red_line {
    position: absolute;
    bottom: 10px;
    left: 134px;
    width: 36px;
    height: 2px;
    border-bottom: 2px solid $color-secondary;
  }

  .news_block {
  }

  .news_text_block {
    padding-top: 16px;
    padding-left: 90px;
  }

  .news_title {
    font-family: $secondary-font;
    font-size: 28px;
    line-height: 36px;
    color: $color-secondary;
  }

  #news_img {
    border-radius: 0px;
  }

  .card__media{
    box-shadow: 5px 10px 18px black;
  }

  .news_red_line:after {
    content: "";
    display: block;
    width: 100%;
    height: 3px;
    margin: -5px auto;
    border-bottom: 2px solid $color-secondary;
  }

  .news_public_date {
    font-family: $primary-font;
    font-size: 11px;
    font-weight: 600;
    color: $color-info;
    margin-top: 24px;
  }

  .new_descr_snippet {
    font-family: $primary-font;
    font-size: 12px;
    font-weight: 600;
    color: white;
    margin-top: 14px;
  }

  #news_pagination {
    margin-bottom: 20px;
  }

  @media only screen and (max-width: $xs-screen) {
    .news_text_block {
      padding-left: 0;
    }
    #filter_block {
      margin-right: 40px;
    }
  }
</style>
