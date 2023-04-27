<template>
  <div class="keyboard_box">
    <div class="vnp-input-box-outer">
      <vnp_input_box
        ref="inputBox"
        v-model="val"
        editable
        @activeChange="handleActiveChange"
      ></vnp_input_box>
    </div>

    <div class="vnp-keys">
      <div class="vnp-keys-row" v-for="(item, index) in list" :key="index">
        <div
          class="vnp-btn-key-wrapper"
          v-for="(val, index) in item"
          :key="index"
          :class="{
            'vnp-del-wrapper': val === 'del',
            'vnp-type-wrapper': val === 'type',
          }"
        >
          <el-button
            v-if="val === 'type'"
            class="vnp-btn-key"
            @click="handleChangeType"
          >
            <span v-if="type === 'cn'"
              >中/<span class="vnp-smaller">英</span></span
            >
            <span v-else><span class="vnp-smaller">中</span>/英</span>
          </el-button>

          <el-button
            v-else-if="val === 'del'"
            class="vnp-btn-key"
            @click="handleDel"
            type="primary"
            icon="el-icon-delete"
          >
            <!-- <svg
              viewBox="0 0 32 22"
              xmlns="http://www.w3.org/2000/svg"
              class="vnp-delete-icon"
            >
              <path
                d="M28.016 0A3.991 3.991 0 0132 3.987v14.026c0 2.2-1.787 3.987-3.98 3.987H10.382c-.509 0-.996-.206-1.374-.585L.89 13.09C.33 12.62 0 11.84 0 11.006c0-.86.325-1.62.887-2.08L9.01.585A1.936 1.936 0 0110.383 0zm0 1.947H10.368L2.24 10.28c-.224.226-.312.432-.312.73 0 .287.094.51.312.729l8.128 8.333h17.648a2.041 2.041 0 002.037-2.04V3.987c0-1.127-.915-2.04-2.037-2.04zM23.028 6a.96.96 0 01.678.292.95.95 0 01-.003 1.377l-3.342 3.348 3.326 3.333c.189.188.292.43.292.679 0 .248-.103.49-.292.679a.96.96 0 01-.678.292.959.959 0 01-.677-.292L18.99 12.36l-3.343 3.345a.96.96 0 01-.677.292.96.96 0 01-.678-.292.962.962 0 01-.292-.68c0-.248.104-.49.292-.679l3.342-3.348-3.342-3.348A.963.963 0 0114 6.971c0-.248.104-.49.292-.679A.96.96 0 0114.97 6a.96.96 0 01.677.292l3.358 3.348 3.345-3.348A.96.96 0 0123.028 6z"
                fill="currentColor"
              ></path>
            </svg> -->
          </el-button>

          <el-button
            v-else
            class="vnp-btn-key"
            :class="{
              'vnp-btn-empty': !val,
            }"
            @click="handleClickKey(val)"
          >
            {{ val }}
          </el-button>
        </div>
      </div>
    </div>
    <div class="vnp-header">
      <button type="button" class="vnp-btn-finish" @click="finishBtn">
        完成
      </button>
    </div>
  </div>
</template>

<script>
import vnp_input_box from "./vnp-input-box.vue";
export default {
  components: {
    vnp_input_box,
  },
  props: {
    show: {
      type: Boolean,
      default: false,
    },
    value: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      val: this.value,
      type: "cn",
      cn: [
        ["京", "沪", "鄂", "川", "渝", "粤", "闽", "晋", "黑", "津"],
        ["浙", "豫", "赣", "贵", "青", "琼", "宁", "吉", "蒙", "冀"],
        ["苏", "皖", "桂", "云", "陕", "甘", "藏", "新", "辽", "挂"],
        ["type", "使", "领", "港", "澳", "警", "学", "del"],
      ],
      en: [
        ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0"],
        ["Q", "W", "E", "R", "T", "Y", "U", "O", "P"],
        ["A", "S", "D", "F", "G", "H", "J", "K", "L"],
        ["type", "Z", "X", "C", "V", "B", "N", "M", "del"],
      ],
    };
  },
  computed: {
    visible: {
      set(val) {
        this.$emit("update:show", val);
      },
      get() {
        return this.show;
      },
    },
    list() {
      return this.type === "en" ? this.en : this.cn;
    },
  },
  watch: {
    show() {
      if (this.show) {
        this.val = this.value;
      }
    },
  },
  methods: {
    handleChangeType() {
      this.type = this.type === "en" ? "cn" : "en";
    },
    handleClickKey(val) {
      if (val) {
        this.$refs.inputBox.setValue(val);
      }
    },
    handleActiveChange(activeIndex) {
      if (activeIndex === 0) {
        this.type = "cn";
      } else {
        this.type = "en";
      }
    },
    handleDel() {
      this.$refs.inputBox.del();
    },
    finishBtn() {
      // console.log(this.val, 1111);
      const regex =
        /^([京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}(([0-9]{5}[DF])|([DF]([A-HJ-NP-Z0-9])[0-9]{4})))|([京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领A-Z]{1}[A-Z]{1}[A-HJ-NP-Z0-9]{4}[A-HJ-NP-Z0-9挂学警港澳]{1})$/;
      if (!regex.test(this.val)) {
        this.$message({
          message: "请输入正确的车牌号",
          type: "warning",
        });
        return;
      }
      console.log("输入正确");
      this.$emit("carName", this.val);
      this.visible = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.keyboard_box {
  position: fixed;
  position: relative;
  float: left;
  z-index: 99999;
  width: 14rem;
  background-color: #fff;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 1px solid #eaeaea;
  border-radius: 0.3rem;
  padding: 0 0.2rem 0.2rem 0.2rem;
  line-height: 0;
}
.keyboard_box:before {
  box-sizing: content-box;
  width: 0px;
  height: 0px;
  position: absolute;
  top: -9px;
  left: 35px;
  padding: 0;
  border-bottom: 5px solid #fff;
  border-top: 5px solid transparent;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  display: block;
  content: "";
  z-index: 12;
}
.keyboard_box:after {
  box-sizing: content-box;
  width: 0px;
  height: 0px;
  position: absolute;
  top: -10px;
  left: 35px;
  padding: 0;
  border-bottom: 5px solid #eaeaea;
  border-top: 5px solid transparent;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  display: block;
  content: "";
  z-index: 10;
}
.vnp-header {
  height: 0.92rem;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-top: 0.3rem;
  .vnp-btn-finish {
    height: 100%;
    color: #fff;
    font-size: 0.12rem;
    background-color: #009d78;
    border: none;
    cursor: pointer;
    width: 65%;
    border-radius: 0.8rem;
  }
}
// 键盘样式
.vnp-keys {
  // padding: 0.06rem;
  width: 12.8rem;
  background: #f2f3f5;
  .vnp-keys-row {
    display: flex;
    justify-content: center;
    .vnp-btn-key-wrapper {
      flex: 0 1 calc((100% - 0.12rem * 10) / 10);
      padding: 0.06rem;
      box-sizing: content-box;
      &.vnp-del-wrapper,
      &.vnp-type-wrapper {
        flex: 1;
        &.vnp-type-wrapper {
          .vnp-smaller {
            color: #999;
            font-size: 0.24rem;
          }
        }
      }
      .vnp-btn-key {
        padding: 0;
        width: 100%;
        font-size: 15px;
        transform: scale(0.7);
        height: 1.5rem;
        border-radius: 0.08rem;
      }

      .vnp-btn-empty {
        background: transparent;
        border: none;
      }

      .vnp-delete-icon {
        width: 0.8rem;
        vertical-align: middle;
      }
    }
  }
}

.el-button--default {
  background-color: rgb(236, 240, 248);
  border: 0.02rem solid #ebedf0;
}
</style>
