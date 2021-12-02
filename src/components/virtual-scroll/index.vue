<template>
  <div class="table-container">
    <div class="header-container">
      <div class="row header">
        <div
          v-for="(col) in cols"
          class="cell"
          :key="col.field"
          :class="$style.headerClasses"
          :style="{width: col.width}"
        >
          <slot :name="`column-${col.field}`" :data="col">
            {{ col.label }}
          </slot>
        </div>
      </div>
    </div>

    <RecycleScroller
      class="body"
      :items="rows"
      :item-size="63"
      :buffer="2000"
      key-field="id"
      v-slot="{ item }"
    >
      <Item :item="item" />
    </RecycleScroller>
  </div>
</template>

<script>
import Item from './item';

export default {
  name: 'VirtalScrollWrapper',
  components: { Item },
  async created() {
    const res = await fetch(`https://jsonplaceholder.typicode.com/comments`);
    this.rows = await res.json();
  },
  data() {
    return {
      rows: [],
      cols: [
        {
          field: "id",
          label: "ID",
          width: "160px",
        },
        {
          field: "postId",
          label: "Post ID",
          width: "200px",
        },
        {
          field: "email",
          label: "Email",
          width: "300px",
        },
        {
          field: "name",
          label: "Name",
          width: "620px",
        },
      ]
    };
  },
};
</script>

<style module>
  .scroller {
    text-align: left;
    margin: 24px;
  }
  .headerClasses {
    display: inline-flex;
    padding: 16px;
    background: #c7cbcb;
    align-items: center;
    color: #2c3e50;
    font-weight: 700;
  }
  .vue-recycle-scroller {
    max-height: 400px;
    width: 500px;
  }
  .row {
    display: flex;
    align-items: center;
  }
</style>
