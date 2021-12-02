<script>
export default {
  name: 'item',
  props: {
    item: {
      type: Object,
      default: () => ({})
    }
  },
  render() {
    const { item, $style } = this;
    return (
      <div class={$style.item}>
        <span class={$style.id}>
          {item.id}
        </span>
        <span class={$style.postId}>
          {item.postId}
        </span>
        <span class={$style.email}>
          {item.email}
        </span>
        <span class={$style.name}>
          {item.name}
        </span>
      </div>
    );
  }
};
</script>

<style module>
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