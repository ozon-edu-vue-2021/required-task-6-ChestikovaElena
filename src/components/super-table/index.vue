<template>
  <div>
    <div class="control">
      <label>
        <input type="radio" value="default" name="feature" checked @change="checkedHandler">
        Таблица с сортировкой и фильтром
      </label>
      <label>
        <input type="radio" value="paging" name="feature" @change="checkedHandler">
        Использовать пагинацию
      </label>
      <label>
        <input type="radio" value="scroll" name="feature" @change="checkedHandler">
        Использовать бесконечный скролл
      </label>
      <label>
        <input type="radio" value="virtualScroll" name="feature" @change="checkedHandler">
        Использовать виртуальный скролл
      </label>
      <label v-if="feature === 'virtualScroll'">
        Тип компонента скролла:
        <select
          value="component"
          v-model="component"
          @change="componentHandler"
        >
          <option value="StatefulLi">Обычный компонент</option>
          <option value="FunctionalLi">Функциональный компонент</option>
        </select>
        <button @click="rendered = true">Нарисовать таблицу</button>
      </label>
    </div>
    <div v-if="feature !== 'virtualScroll'">
      <Table
        :rows="rows"
        :total-pages="100"
        :current-page="currentPage"
        :feature="feature"
    
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
    <div v-else>
      <VirtalScrollWrapper
        :componentType="component"
        :rendered="rendered"
      />
    </div>
  </div>
</template>

<script>
import Table from './table';
import Column from './column';
import VirtalScrollWrapper from '../virtual-scroll';

export default {
  name: 'SuperTable',
  components: {
    Column,
    Table,
    VirtalScrollWrapper
  },
  created() {
    this.getAllPage();
  },
  data() {
    return {
      feature: 'default',
      rows: [],
      currentPage: 1,
      getPageFunction: this.getPage,
      component: "FunctionalLi",
      rendered: false,
    };
  },
  methods: {
    async getAllPage() {
      const res = await fetch(`https://jsonplaceholder.typicode.com/comments`);
      this.rows = await res.json();
    },
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
        this.feature = 'paging';
        this.currentPage = 1;
        this.getPageFunction = this.getPage;
      } else if (event.target.value === 'scroll') {
        this.blockingPromise = this.getPage(1);
        this.feature = 'scroll';
        this.getPageFunction = this.infGetPage;
      } else if (event.target.value === 'virtualScroll') {
        this.getAllPage();
        this.feature = 'virtualScroll';
      } else {
        this.getAllPage();
        this.feature = 'default';
      }
    },
    componentHandler() {
      this.rendered = false;
    }
  }
};
</script>
