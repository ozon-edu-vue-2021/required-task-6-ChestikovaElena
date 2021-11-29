<template>
  <div>
    <div class="control">
      <div>
        <input type="radio" value="paging" name="feature" checked @change="checkedHandler">
        <label>Использовать пагинацию</label>
      </div>
      <div>
        <input type="radio" value="scroll" name="feature" @change="checkedHandler">
        <label>Использовать бесконечный скролл</label>
      </div>
    </div>
    <Table
      :rows="rows"
      :total-pages="100"
      :current-page="currentPage"
      :static-paging="staticPaging"
  
      @getPage="getPageFunction"
    >
      <Column prop="id" title="ID" />
      <Column prop="postId" title="Post ID" />
  
      <Column prop="email">
        <template #title>
          <b>Email</b>
        </template>
        <template #body="{ row }">
          <a :href="`mailto:${row.email}`">{{ row.email }}</a>
        </template>
      </Column>
  
      <Column prop="name" title="Name" />
    </Table>
  </div>
</template>

<script>
import Table from './table';
import Column from './column';

export default {
  name: 'BaseTable',
  // props: {
  //   feature: {
  //     type: String,
  //     default: 'paging'
  //   }
  // },
  components: {
    Column,
    Table,
  },
  created() {
    this.blockingPromise = this.getPage(1);
  },
  data() {
    return {
      feature: 'paging',
      rows: [],
      currentPage: 1,
      staticPaging: true,
      getPageFunction: this.getPage,
    };
  },
  // computed: {
  //   staticPaging() {
  //     if (this.feature === 'paging') {
  //       return true
  //     } else {
  //       return false
  //     }
  //   },
  //   getPageFunction() {
  //     if (this.feature === 'paging') {
  //       return this.getPage
  //     } else {
  //       return this.infGetPage
  //     }
  //   },
  // },
  methods: {
    async getPage(number) {
      const res = await fetch(`https://jsonplaceholder.typicode.com/comments?postId=${number}`);
      this.rows = await res.json();
      this.currentPage = number;
    },
    async infGetPage() {
      this.blockingPromise && await this.blockingPromise;
      const res = await fetch(`https://jsonplaceholder.typicode.com/comments?postId=${this.currentPage + 1}`);
      const newRows = await res.json();
      this.rows = [...this.rows, ...newRows];
      this.currentPage++;
    },
    checkedHandler(event) {
      this.rows = [];
      if (event.target.value === 'paging') {
        this.getPage(1);
        this.currentPage = 1;
        this.staticPaging = true;
        this.getPageFunction = this.getPage;
      } else {
        this.blockingPromise = this.getPage(1);
        this.staticPaging = false;
        this.getPageFunction = this.infGetPage;
      }
    }
  }
};
</script>
