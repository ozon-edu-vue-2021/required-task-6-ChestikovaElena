<script lang="jsx">
import { orderBy } from 'lodash/collection';
import FilterDropdown from './filter-dropdown';
import TablePaginator from './table-paginator';
import DotsLoaderIcon from './dost-loader.svg';

export default {
  name: 'Table',
  props: {
    rows: {
      type: Array,
      default: () => []
    },
    totalPages: {
      type: Number,
      default: 0
    },
    currentPage: {
      type: Number,
      default: 0
    },
    feature: {
      type: String,
      default: "default"
    }
  },
  data() {
    return {
      sortProps: {},
      filterProps: {},
    }
  },
  computed: {
    sortedRows() {
      let res = this.rows;

      if (this.feature === 'default') {
        if (Object.keys(this.sortProps).length === 0
          && Object.keys(this.filterProps).length === 0) {
          res = this.rows;
        }
        
        if (Object.keys(this.sortProps).length) {
          const props = Object.keys(this.sortProps);
          const sortDirection = Object.keys(this.sortProps).map(key => this.sortProps[key]);
          res = orderBy(this.rows, props, sortDirection);
        }
        
        if(Object.keys(this.filterProps).length !== 0) {
          Object.keys(this.filterProps).map((filterPropName) => 
            res = res.filter(row => String(row[filterPropName]).search(this.filterProps[filterPropName]) > -1)
          );
        }
      }
      
      return res;
    }
  },
  methods: {
    toggleSort(prop) {
      const sortDirection = (this.sortProps[prop] === 'desc' || !this.sortProps[prop]) ? 'asc' : 'desc';
      this.$set(this.sortProps, prop, sortDirection);
    },
    closeSort(prop) {
      this.$delete(this.sortProps, prop);
    },
    closeFilterTooltip(prop = '') {
      this.$delete(this.filterProps, prop);
    },
    setFilterText( prop, e ) {
      const value = e.target.value;
      this.$set(this.filterProps, prop, value);
    },
    renderHead(h, columnsOptions) {
      const {$style, sortProps, feature, filterProps, closeFilterTooltip } = this;
      
      return columnsOptions.map((column) => {
        const renderedTitle = column.scopedSlots.title ? column.scopedSlots.title() : column.title;
        let sortIcon = 'sort';
        
        if (Object.keys(sortProps).includes(column.prop)) {
          sortIcon = sortProps[column.prop] === 'asc' ? 'sort-amount-down' : 'sort-amount-up';
        }
        
        return (
          <th key={column.prop} class={$style.headerCell}>
            <div class={$style.headerCellContent}>
              <span>{renderedTitle}</span>
              <div
                class={$style.sortIconWrapper}
                v-show={feature === 'default'}
              >
                <font-awesome-icon
                  class={$style.sortIcon}
                  icon={sortIcon}
                  on={{ click: () => this.toggleSort(column.prop) }}
                />
                <font-awesome-icon
                  v-show={sortIcon !== 'sort'}
                  icon="times"
                  class={$style.closeIcon}
                  on={{ click: () => this.closeSort(column.prop) }}
                />
              </div>
              
              <FilterDropdown
                v-show={feature === 'default'}
                columnProp={column.prop}
                filterText={filterProps[column.prop]}
                closeFilterTooltip={closeFilterTooltip}
                on={{
                  setFilterText: () => this.setFilterText(column.prop, event),
                }}
              />
            </div>
          </th>
        );
      });
    },
    renderRows(h, columnsOptions) {
      return this.sortedRows.map((row, index) => {
        return (
          <tr key={row.id || index}>
            {...this.renderColumns(h, row, columnsOptions)}
          </tr>
        )
      });
    },
    renderColumns(h, row, columnsOptions) {
      return columnsOptions.map((column) => {
        return (
          <td key={column.prop} class={this.$style.cell}>
            {column.scopedSlots.body ? column.scopedSlots.body({ row }) : row[column.prop]}
          </td>
        );
      });
    },
    getColumnOptions() {
      return this.$slots.default.
        filter(item => item.componentOptions && item.componentOptions.tag === 'Column').
        map(column =>
          Object.assign({}, column.componentOptions.propsData, {
              scopedSlots: column.data.scopedSlots || {}
            }
          )
        );
    },
    renderInfPager() {
      const directives = [
        {
          name: 'detect-viewport',
          value: {
            callback: this.$listeners.getPage
          }
        }
      ];
      
      const style = {
        background: `url("${DotsLoaderIcon}") no-repeat center`
      };
      return (
        <div {...{ class: this.$style.infPager, style, directives }} />
      );
    }
  },
  render(h) {
    const { $style, feature, totalPages, currentPage, $listeners } = this;
    const { getPage } = $listeners;
    const columnsOptions = this.getColumnOptions();
    const columnsHead = this.renderHead(h, columnsOptions);
    const rows = this.renderRows(h, columnsOptions);

    return (
      <div>
        <table class={$style.table}>
          <thead>{...columnsHead}</thead>
          <tbody>
            {...rows}
          </tbody>
        </table>

        {feature === 'paging' 
          ? <TablePaginator totalPages={totalPages} currentPage={currentPage} on={{ getPage: getPage }} />
          : feature === 'scroll'
            ? this.renderInfPager()
            : null
        }
      </div>
    );
  }
};
</script>

<style module>
  .table {
    border-spacing: 0;
    margin: 8px;
    width: 100%;
  }
  .cell {
    text-align: left;
    border-bottom: 1px solid #c8cacc;
    padding: 1rem 1rem;
  }
  .headerCell {
    composes: cell;
    background: #c7cbcb;
  }
  .headerCellContent {
    display: flex;
    align-items: center;
  }
  .sortIconWrapper {
    position: relative;
    width: 32px;
  }
  .sortIcon {
    margin-left: 8px;
    margin-right: 8px;
  }
  .sortIcon:hover {
    cursor: pointer;
  }
  .closeIcon {
    width: 12px;
    height: 12px;
    position: absolute;
    top: -10px;
    right: 0px;
  }
  .closeIcon:hover {
    cursor: pointer;
  }
  .infPager {
    width: 100%;
    height: 32px;
  }

</style>
