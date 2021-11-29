<script>
export default {
  name: "FilterDropdown",
  props: {
    columnProp: {
      type: String,
      default: ''
    },
    shown: {
      type: Boolean,
      default: false
    },
    filterText: {
      type: String,
      default: ''
    }
  },
  methods: {
    hideDropdown() {
      return this.isDropdownOpen = false;
    },
    openFilter() {
      this.isDropdownOpen = true;
      this.openFilterTooltip()
    }
  },
  render() {
    const { $style, shown, filterText, $listeners } = this;
    const { openFilterTooltip, setFilterText, closeFilterTooltip } = $listeners;
    
    return (
      <div>
        <div class={$style.selector}>
          <font-awesome-icon
            icon="filter"
            class={$style.iconFilter}
            on={{ click: openFilterTooltip }}
          />

            <div class={$style.inputWrapper} slot="popper" v-show={ shown }>
              <input
                class={$style.input}
                type="text"
                domProps={{ value: filterText }}
                on={{ input: setFilterText }}
              />

              <font-awesome-icon
                icon="times"
                class={$style.iconClose}
                on={{ click: closeFilterTooltip }}
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
