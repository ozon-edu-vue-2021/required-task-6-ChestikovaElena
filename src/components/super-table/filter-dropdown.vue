<script>
export default {
  name: "FilterDropdown",
  props: {
    columnProp: {
      type: String,
      default: ''
    },
    filterText: {
      type: String,
      default: ''
    },
    closeFilterTooltip: {
      type: Function,
      default: () => {}
    }
  },
  data() {
    return {
      isDropdownOpen: false,
    }
  },
  methods: {
    hideDropdown() {
      this.isDropdownOpen = false;
      this.closeFilterTooltip(this.columnProp);
    },
    openFilter() {
      this.isDropdownOpen = true;
    }
  },
  render() {
    const { $style, isDropdownOpen, filterText, $listeners } = this;
    const { setFilterText } = $listeners;
    
    return (
      <div>
        <div class={$style.selector}>
          <font-awesome-icon
            icon="filter"
            class={$style.iconFilter}
            on={{ click: () => this.openFilter() }}
          />

            <div class={$style.inputWrapper} slot="popper" v-show={isDropdownOpen}>
              <input
                class={$style.input}
                type="text"
                domProps={{ value: filterText }}
                on={{ input: setFilterText }}
              />

              <font-awesome-icon
                icon="times"
                class={$style.iconClose}
                on={{ click: () => this.hideDropdown() }}
              />
            </div>
        </div>
      </div>
    );
  }
}
</script>

<style module>
.selector {
  display: flex;
  flex-direction: column;
  position: relative;
  width: 80px;
}
.iconFilter {
  margin-left: 8px;
  margin-right: 24px;
  cursor: pointer;
}
.inputWrapper {
  position: absolute;
  top: 18px;
}
.input {
  width: 70px;
}
.iconClose {
  cursor: pointer;
  position: absolute;
  top: -15px;
}

</style>
