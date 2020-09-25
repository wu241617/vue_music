<template>
  <div class="index-container">
    <div class="nav" >
		<img src="../../public/music_3gif.gif" alt="图片加载失败!" id="music_3">
      <ul>
        <li>
          <router-link to="/discovery">
            <span class="iconfont icon-find-music"></span>
            发现音乐
          </router-link>
        </li>
        <li>
          <router-link to="/playlists">
            <span class="iconfont icon-music-list"></span>
            推荐歌单
          </router-link>
        </li>
        <li>
          <router-link to="/songs">
            <span class="iconfont icon-music"></span>
            最新音乐
          </router-link>
        </li>
        <li>
          <router-link to="/mvs">
            <span class="iconfont icon-mv"></span>
            最新MV
          </router-link>
        </li>
		<span id="jianjie">
			<img src="../../public/icon_1.jpg" alt="图片加载失败!">
		</span>
      </ul>
    </div>
    <div class="main">
      <router-view></router-view>
    </div>
    <div class="player">
      <audio controls autoplay :src="url" loop></audio>
    </div>
  </div>
</template>

<script>
export default {
  name: 'index',
  data() {
    return {
      activeIndex: 0,
      url:""
    };
  }
};
</script>

<style lang="css">
@keyframes move{
	from{transform: rotate(0deg);}
	to{transform: rotate(360deg);}
}
</style>
