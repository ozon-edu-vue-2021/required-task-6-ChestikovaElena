<template>
  <div
    v-if="rendered"
    class="table-container"
  >
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
      v-if="componentType === 'StatefulLi'"
      class="body"
      :items="rows"
      :item-size="63"
      :buffer="2000"
      key-field="id"
      v-slot="{ item }"
    >
      <StatefulLi :item="item" />
    </RecycleScroller>
    <RecycleScroller
      v-else
      class="body"
      :items="rows"
      :item-size="63"
      :buffer="2000"
      key-field="id"
      v-slot="{ item }"
    >
      <FunctionalLi :item="item" />
    </RecycleScroller>
  </div>
</template>

<script>
import StatefulLi from './statfulLi';
import FunctionalLi from './functionalLi';

export default {
  name: 'VirtalScrollWrapper',
  components: { StatefulLi, FunctionalLi },
  props: {
    componentType: {
      type: String,
      default: "StatefulLi"
    },
    rendered: {
      type: Boolean,
      default: false
    }
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
      ],
    };
  },
  async created() {
    const res = await fetch(`https://jsonplaceholder.typicode.com/comments`);
    this.rows = await res.json();
  },
  beforeUpdate() {
    console.time();
  },

  updated() {
    console.log(`Time for render ${this.componentType}: `);
    console.timeEnd();
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
