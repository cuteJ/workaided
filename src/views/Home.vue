<template>
  <div class="app-container">
    <div class="nav-list">

      <div class="nav-item" v-for="i in 12">
        <div class="nav-box" @click="openLink('https://www.baidu.com')">
          <img src="@/assets/links/baidu.png">

          <div class="nav-box-title text">百度</div>
          <p class="nav-box-desc text">百度一下，你就知道</p>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
  import openWindow from "@/utils/openWindow";

  export default {
    name: 'Home',
    data() {
      return {}
    },
    methods: {
      openLink(link) {
        openWindow(link, '百度', 600, 700)
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style rel="stylesheet/scss" lang="scss" scoped>

  @import '../styles/shadow.scss';

  .nav-list {
    display: flex;
    flex-wrap: wrap;

    .nav-item {
      padding: 10px;
    }

    img {
      width: 150px;
    }

    .nav-box {
      cursor: pointer;
      padding: 10px;
      position: relative;

      text-align: center;
      background-color: white;
      width: 200px;
      border-radius: 4px;

      .nav-box-title {
        margin-top: 10px;
        font-weight: 500;
        font-size: 16px;
        line-height: 34px;
        color: #323233;
      }

      .nav-box-desc {
        font-size: 12px;
        line-height: 20px;
        color: #969799;
      }

      .text:hover {
        color: #177EE6;
      }

      &:hover {
        opacity: .8;
        top: -8px;
        @include shadow-down();
      }
    }


  }

</style>

