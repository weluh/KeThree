<template>
  <div id="app">
   
    <router-view/>
    
  </div>
</template>

<style lang="less">
  body{
    background-color: #fff;
  }
  #app {
    font-family: "PingFang SC",Arial;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    font-size: 16px;
    color: #646566;
    // margin-top: 26px;
  }
  .auto-img{
    width: 100%;
    display: block;
  }
  .clearfix::after{
    content: "";
    display: block;
    clear: both;
  }
  .fl{
    float: left;
  }
  .fr{
    float: right;
  }

  .one-text{
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
</style>
