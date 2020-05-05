<template>
  <div id="app">
    <div class="nav">
      <div class="search">
        <input placeholder="请输入....." type="search"/>
        <button :class="{jello: active}" @click="handleSearch">搜索</button>
      </div>
    </div>
    <router-view/>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        active: false
      }
    },
    methods: {
      handleSearch() {
        this.active = true;
      }
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss">

  .nav {
    height: 200px;
    padding-top: 50px;
  }

  .jello {
    animation: jello 2s ease-in-out;
  }

  .search {
    text-align: center;

    button {
      display: inline-block;
      line-height: 1;
      white-space: nowrap;
      cursor: pointer;
      background: #3A71A8;
      color: white;
      -webkit-appearance: none;
      text-align: center;
      box-sizing: border-box;
      outline: none;
      border: none;
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

    input[type='search'] {
      width: 300px;
      min-height: 40px;

      padding: 8px;

      outline: none;
      border: 1px solid #c8c9cc;
      font-size: 14px;
      border-radius: 4px;

      &:focus {
        border: 1px solid #177EE6;
      }
    }
  }

  ::-webkit-input-placeholder { /* Chrome/Opera/Safari */
    text-indent: 8px;
    color: #969799;
  }

  ::-moz-placeholder { /* Firefox 19+ */
    text-indent: 8px;
    color: #969799;
  }

  :-ms-input-placeholder { /* IE 10+ */
    text-indent: 8px;
    color: #969799;
  }

  :-moz-placeholder { /* Firefox 18- */
    text-indent: 8px;
    color: #969799;
  }

</style>
