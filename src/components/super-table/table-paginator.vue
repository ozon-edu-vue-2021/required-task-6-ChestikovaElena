<script lang="jsx">
export default {
  name: 'TablePaginator',
  props: {
    totalPages: {
      type: Number,
      default: 0
    },
    currentPage: {
      type: Number,
      default: 0
    }
  },
  computed: {
    shownPagesNumbers() {
      const { currentPage, totalPages } = this;

      if (currentPage <= 3) {
        if (totalPages < 5) {
          return Array(totalPages).fill(null).map(index => index + 1);
        }

        return [1, 2, 3, 4, 5];
      }

      if (currentPage >= totalPages - 2) {
        return [
          totalPages - 4,
          totalPages - 3,
          totalPages - 2,
          totalPages - 1,
          totalPages
        ];
      }

      return [
        currentPage - 2,
        currentPage - 1,
        currentPage,
        currentPage + 1,
        currentPage + 2
      ];
    }
  },
  render() {
    const { $style, shownPagesNumbers, currentPage, totalPages, $listeners } = this;
    const { getPage } = $listeners;

    return (
      <div class={$style.pagination}>
        <span
          class={[
            $style.control, {
              [$style.disabled]: currentPage === 1
            }
          ]}
          on={{ click: () => getPage(1) }}
        >
          {'<<'}
        </span>
        <span
          class={[
            $style.control, {
              [$style.disabled]: currentPage === 1
            }
          ]}
          on={{ click: () => getPage(currentPage - 1) }}
        >
          {'<'}
        </span>
        {...shownPagesNumbers.map(
          number => (
            <span
              class={[
                $style.control, {
                  [$style.active]: currentPage === number,
                }
              ]}
              on={{ click: () => getPage(number) }}
            >
              {number}
            </span>
          )
        )}
        <span
          class={[
            $style.control, {
              [$style.disabled]: currentPage === totalPages
            }
          ]}
          on={{ click: () => getPage(currentPage + 1) }}
        >
          {'>'}
        </span>
        <span
          class={[
            $style.control, {
              [$style.disabled]: currentPage === totalPages
            }
          ]}
          on={{ click: () => getPage(totalPages) }}
        >
          {'>>'}
        </span>
      </div>
    );
  }
};
</script>

<style module>
  .pagination {
    height: 48px;
    width: 100%;
    border-bottom: 1px solid #c8cacc;
    display: flex;
    align-items: center;
    margin-left: 8px;
  }
  .control {
    width: 28px;
    height: 28px;
    margin-right: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .control:last-of-type {
    margin-right: 0;
  }
  .control:hover {
    cursor: pointer;
  }
  .control.active {
    color: cornflowerblue;
    background: ivory;
    border: 1px solid #c8cacc;
    border-radius: 6px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .control.disabled {
    color: lightgrey;
    cursor: auto;
    pointer-events: none;
  }
</style>
