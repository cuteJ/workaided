<template>
  <div class="app-container">
    <div class="nav-list">
      <div class="nav-item" v-for="item in links">
        <div class="nav-box" @click="openLink(item)">
          <div style="height: 150px; vertical-align: middle; text-align: center">
            <img :src="item.logo">
          </div>
          <button class="write-btn" @click="toWrite(item, $event)">去写作</button>
          <div class="nav-box-title text">{{item.title}}</div>
          <p class="nav-box-desc text">{{item.desc}}</p>
        </div>
      </div>
    </div>
    <!--    <link-node-->
    <!--      ref="diag"-->
    <!--      :model-data="diagramData"-->
    <!--      style="border: solid 1px black; width:100%; height: 100%;"-->
    <!--      @model-changed="modelChanged"-->
    <!--      @changed-selection="nodeChanged"-->
    <!--      @changed-link="linkChanged">-->
    <!--    </link-node>-->
  </div>
</template>

<script>
  import openWindow from "@/utils/openWindow";
  import LinkNode from "@/components/LinkNode";
  import {DiagramData, WriterLink} from "@/utils/localData";

  export default {
    name: 'Home',
    components: {
      LinkNode
    },
    data() {
      return {
        diagramData: DiagramData,
        diagramDataJson: '',
        links: WriterLink
      }
    },
    methods: {

      openLink(item, e) {
        // window.location.href = item.link;
        window.open(item.link, "_blank");
        // openWindow(item.url, item.title, 600, 700)
      },
      toWrite(item, e) {
        e.preventDefault();
        e.stopPropagation();
        window.open(item.wLink, "_blank");
        return false;
      },

      /** ********************模型API************************/
      // 返回图模型对象
      // model() {
      //   return this.$refs.diag.model()
      // },
      // 监听模型图变动
      modelChanged(e) {
        if (e.isTransactionFinished) {
          this.diagramDataJson = e.model.toJson()
        }
      },
      // 监听当前选中节点
      nodeChanged(e) {
        const node = e.diagram.selection.first()
        if (node instanceof window.go.Node) {
        } else {
        }
      },
      // 监听当前选中规则
      linkChanged(e) {
        const link = e.diagram.selection.first()
        if (link instanceof window.go.Link) {
        } else {
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style rel="stylesheet/scss" lang="scss" scoped>

  @import '../styles/shadow.scss';

  .write-btn {
    display: inline-block;
    line-height: 1;
    white-space: nowrap;
    cursor: pointer;
    /*background: #3A71A8;*/
    /*color: white;*/
    -webkit-appearance: none;
    text-align: center;
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    font-size: 14px;
    border-radius: 4px;

    width: 80px;
    height: 40px;

    &:hover {
      opacity: .8;
    }
  }

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

