<template>
  <div class="mv-container">
    <div class="mv-wrap">
      <h3 class="title">mv详情</h3>
      <!-- mv -->
      <div class="video-wrap">
        <video controls :src="mvUrl"></video>
      </div>
      <!-- mv信息 -->
      <div class="info-wrap">
        <div class="singer-info">
          <div class='avatar-wrap'>
            <img :src="artistCover" alt="" />
          </div>
          <span class="name">{{ artistName }}</span>
        </div>
        <div class="mv-info">
          <h2 class="title">{{ mvName }}</h2>
          <span class="date">发布：{{ publishTime }}</span>
          <span class="number">播放：{{ playCount | formatCount }}次</span>
          <p class="desc">{{ desc }}</p>
        </div>
      </div>
      <!-- 热门评论 -->
      <div v-if="hotComments.length != 0" class="comment-wrap">
        <p class="title">
          热门评论<span class="number">({{ hotComments.length }})</span>
        </p>
        <div
          class="comments-wrap"
          v-for="item in hotComments"
          :key="item.commentId"
        >
          <div class="item">
            <div class="icon-wrap">
              <img :src="item.user.avatarUrl" alt="" />
            </div>
            <div class="content-wrap">
              <div class="content">
                <span class="name">{{ item.user.nickname }}：</span>
                <span class="comment">{{ item.content }}</span>
              </div>
              <div v-if="item.beReplied.length != 0" class="re-content">
                <span class="name"
                  >{{ item.beReplied[0].user.nickname }}：</span
                >
                <span class="comment">{{ item.beReplied[0].content }}</span>
              </div>
              <div class="date">{{ item.time | formatTime }}</div>
            </div>
          </div>
        </div>
      </div>
      <!-- 最新评论 -->
      <!-- 最新评论 -->
      <div class="comment-wrap" v-if="comments.length != 0">
        <p class="title">
          最新评论<span class="number">({{ total }})</span>
        </p>
        <div class="comments-wrap">
          <div class="item" v-for="item in comments" :key="item.commentId">
            <div class="icon-wrap">
              <img :src="item.user.avatarUrl" alt="" />
            </div>
            <div class="content-wrap">
              <div class="content">
                <span class="name">{{ item.user.nickname }}：</span>
                <span class="comment">{{ item.content }}</span>
              </div>
              <div v-if="item.beReplied.length != 0" class="re-content">
                <span class="name"
                  >{{ item.beReplied[0].user.nickname }}：</span
                >
                <span class="comment">{{ item.beReplied[0].content }}</span>
              </div>
              <div class="date">{{ item.time | formatTime }}</div>
            </div>
          </div>
        </div>
      </div>
      <!-- 分页器 -->
      <el-pagination
        @current-change="handleCurrentChange"
        background
        layout="prev, pager, next"
        :total="total"
        :current-page="page"
        :page-size="5"
      >
      </el-pagination>
    </div>
    <div class="mv-recommend">
      <h3 class="title">相关推荐</h3>
      <div class="mvs">
        <div class="items" v-for="item in simiMV" :key="item.id">
          <div class="item" @click="toMV(item.id)">
            <div class="img-wrap">
              <img :src="item.cover" alt="" />
              <span class="iconfont icon-play"></span>
              <div class="num-wrap">
                <div class="iconfont icon-play"></div>
                <div class="num">{{ item.playCount | formatCount }}</div>
              </div>
              <span class="time">{{ item.duration | formatDuration }}</span>
            </div>
            <div class="info-wrap">
              <div class="name">{{ item.name }}</div>
              <div class="singer">{{ item.artistName }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mvUrl, simiMV, mvComments, mvDetail, artistInfo } from '@/api/mv';
export default {
  name: 'mv',
  data() {
    return {
      // mv地址
      mvUrl: '',
      simiMV: [],
      // 热门评论
      hotComments: [],
      // 普通评论
      comments: [],
      // 分页相关数据
      // 每页数据
      limit: 20,
      // 页码
      page: 1,
      // 总条数
      total: 0,
      // mv的名字
      mvName: '',
      // 播放次数
      playCount: '',
      // 发布时间
      publishTime: '',
      // 描述
      desc: '',
      // 歌手名
      artistName: '',
      // 封面
      artistCover: ''
    };
  },

  methods: {
    // 页码改变
    handleCurrentChange(val) {
      this.page = val;
      this.getComments();
    },
    // 获取评论
    getComments() {
      const { id } = this.$route.query;
      mvComments({ id, offset: (this.page - 1) * 5 }).then(res => {
        // window.console.log(res)
        if (res.hotComments) {
          this.hotComments = res.hotComments;
        }
        this.comments = res.comments;
        this.total = res.total;
      });
    },
    // 获取信息
    getInfo() {
      const { id } = this.$route.query;
      mvUrl({ id }).then(res => {
        this.mvUrl = res.data.url;
      });
      simiMV({ mvid: id }).then(res => {
        // window.console.log(res)
        this.simiMV = res.mvs;
      });
      // 获取评论
      this.getComments();
      // 获取MV详情
      mvDetail({ mvid: id }).then(res => {
        this.mvName = res.data.name;
        this.playCount = res.data.playCount;
        this.publishTime = res.data.publishTime;
        this.desc = res.data.desc;
        artistInfo({
          artistId: res.data.artistId
        }).then(res => {
          // 歌手名
          this.artistName = res.artist.name;
          this.artistCover = res.artist.picUrl;
        });
      });
    },
    // 切换mv
    toMV(id) {
      this.$router.push(`/mv?id=${id}`);
    }
  },
  // 侦听器
  watch: {
    '$route.query.id'() {
      this.page = 1;
      this.getInfo();
    }
  },
  created() {
    this.getInfo();
  }
};
</script>

<style lang="scss"></style>
