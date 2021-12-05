<template functional>
  <div class="item">
    <span class="id">
      {{props.item.id}}
    </span>
    <span class="postId">
      {{props.item.postId}}
    </span>
    <span class="email">
      {{props.item.email}}
    </span>
    <span class="name">
      {{props.item.name}}
    </span>
  </div>
</template>

<script>
export default {
  name: 'item',
  props: {
    item: {
      type: Object,
      default: () => ({})
    }
  },
};
</script>

<style scoped>
  .item {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    height: 55px;
    border-bottom: 1px solid #c8cacc;
    padding: 1rem 1rem;
  }
  .id {
    margin-right: 16px;
    width: 160px;
  }
  .postId {
    margin-right: 16px;
    width: 200px;
  }
  .email {
    margin-right: 16px;
    width: 300px;
  }
  .name {
    margin-right: 16px;
    width: 620px;
  }
</style>
