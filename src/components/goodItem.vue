<template>
  <div class="good-item">
    <div class="head">
      <span class="border"></span>
      <span class="title">{{title}}</span>
      <span class="describe">{{describe}}</span>
    </div>
    <slot></slot>
  </div>
</template>

<script>
  export default {
    props: {
      title: String,
      describe: String,
      moreLink: {
        type: String,
        default: ""
      }
    }
  };
</script>

<style lang="less" scoped>
  .good-item {
    font-size :18px;
    background-color:#fff;
    padding :4px 6px;
    margin-bottom: 6px;

    .head {
      padding :4px 0;

      .border {
        border-left: 3px solid red;
      }

      .title {
        margin-left :6px;
        font-weight :700
      }

      .describe {
        font-size :12px;
        margin-left: 6px;
        color: #8f8f8f;
      }


    }
  }

</style>
