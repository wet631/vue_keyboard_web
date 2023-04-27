<template>
  <div class="vnp-input-box">
    <div class="greenText"><span>新能源</span></div>
    <ul>
      <li
        v-for="(item, index) in val"
        :key="index"
        :class="{ active: activeIndex === index }"
        @click="handleClickItem(index)"
      >
        <span>{{ item }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: "",
    },
    editable: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      val: ["", "", "", "", "", "", "", ""],
      activeIndex: this.editable ? 0 : undefined,
    };
  },
  watch: {
    activeIndex() {
      this.$emit("activeChange", this.activeIndex);
    },
    value: {
      immediate: true,
      handler() {
        if (this.val.join("") === this.value) {
          return;
        }
        const val = this.value.split("");
        if (this.editable) {
          this.activeIndex = val.length;
        }
        while (val.length < 8) {
          val.push("");
        }
        this.val = val;
      },
    },
    val() {
      this.$emit("input", this.val.join(""));
    },
  },
  methods: {
    handleClickItem(index) {
      if (!this.editable) {
        return;
      }
      this.activeIndex = index;
    },
    setValue(val) {
      this.$set(this.val, this.activeIndex, val);
      if (this.activeIndex < 7) {
        this.activeIndex += 1;
      }
    },
    del() {
      this.$set(this.val, this.activeIndex, "");
      if (this.activeIndex > 0) {
        this.activeIndex -= 1;
      }
    },
  },
};
</script>

<style scoped lang="scss">
.vnp-input-box {
  color: #8d8d8d;
  font-size: 0.1rem;
  width: 12.8rem;
  margin-bottom: 0.3rem;
  .greenText {
    width: 15rem;
    text-align: right;
    color: #41d04f;
    font-size: 0.12rem;
    transform: scale(0.7);
    margin: 8px 0;
  }
  ul {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-inline-start: 0;
  }
  li {
    border: 0.02rem solid #eaeaea;
    height: 1.5rem;
    width: 2rem;
    line-height: 1.2rem;
    margin-right: 0.1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 0.4rem;
    &:last-child {
      border-color: #41d04f;
      position: relative;
    }
    &.active {
      color: #1989fa;
      > span {
        height: 100%;
        width: 1rem;
        display: inline-block;
        border-bottom: 0.02rem solid #1989fa;
        // border-color: #41d04f;
      }
    }
  }
}
</style>
