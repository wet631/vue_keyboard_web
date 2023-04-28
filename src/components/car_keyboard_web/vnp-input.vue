<template>
  <div class="main_box">
    <div class="inputBox">
      <!-- <el-input
        v-model="inputNumber"
        placeholder="请输入车牌"
        suffix-icon="el-icon-caret-bottom"
        @focus="showBtn"
        clearable
        :disabled="showInput"
      ></el-input> -->
      <div class="inputMain">
        <input
          type="text"
          placeholder="请输入车牌"
          v-model="inputNumber"
          :disabled="showInput"
          class="vnp_input"
          @click="mainBox"
        />
      </div>
      <div class="iconBox">
        <img
          src="./pic/danchujianpan.png"
          alt=""
          :class="rotateA ? 'picBox' : 'picBox go'"
        />
        <transition name="fade">
          <vnp_keyboard
            v-show="showKeyBoard"
            @carName="carName"
            class="vnp_keyBoard"
            ref="keyboard"
          ></vnp_keyboard>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
import vnp_keyboard from "./vnp-keyboard.vue";
export default {
  data() {
    return {
      inputNumber: "",
      showKeyBoard: false,
      showInput: false,
      rotateA: false,
    };
  },
  components: {
    vnp_keyboard,
  },
  mounted() {
    window.onload = () => {};
  },
  methods: {
    mainBox() {
      const inputElement = document.querySelector(".inputBox");
      const floatingDivElement = this.$refs.keyboard.$el;
      // 获取输入框相对于文档顶部的距离和高度
      const inputTop = inputElement.getBoundingClientRect().top;
      // 获取当前窗口高度
      const windowHeight = document.documentElement.clientHeight;
      // 175 为键盘高度 
      if (windowHeight - inputTop < 175) {
        // 在输入框上方展示键盘
        floatingDivElement.style.top = `${-185}px`;
      } else {
        floatingDivElement.style.top = `50px`;
      }
      this.showKeyBoard = true;
      this.rotateA = true;
    },
    carName(val) {
      this.inputNumber = val;
      this.showKeyBoard = false;
      // this.showInput = false;
      this.rotateA = false;
    },
  },
};
</script>

<style lang="scss" scoped>
.inputBox {
  width: 200px;
  height: 40px;
  // background-color: pink;
  display: flex;
  // flex-direction: column;
  // justify-content: center;
  // align-items: center;
  border-radius: 3px;
  border: 1px solid #c0c3cb;
  .inputMain {
    width: 165px;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    // background-color: pink;
    padding-left: 15px;
    input {
      border: 0; /*清除自带的2px的边框*/
      padding: 0; /*清除自带的padding间距*/
      outline: none; /*清除input点击之后的黑色边框*/
      color: #606266;
      background-color: #fff;
      width: 140px;
      // width: 150px;
      // margin-left: 20px;
    }
  }
  .iconBox {
    width: 35px;
    display: flex;
    position: relative;
    // text-align: center;
    // flex-direction: column;
    justify-content: center;
    align-items: center;
    border-left: 1px solid #c4c3cb;
    .picBox {
      width: 20px;
      height: 20px;
    }
    .vnp_keyBoard {
      position: absolute;
      top: 50px;
      right: -15px;
    }
  }
}
.go {
  transform: rotate(180deg);
  transition: all 0.5s;
}
.aa {
  transform: rotate(180deg);
  transition: all 2s;
}
img {
  transition: transform 0.3s; /* 设置过渡动画 */
}

.rotate {
  /* 点击后附加该类来使图片旋转 */
  transform: rotate(180deg);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease-out;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

input::-webkit-input-placeholder {
  /* WebKit browsers 适配谷歌 */
  color: #c4c3cb;
}
input:-moz-placeholder {
  /* Mozilla Firefox 4 to 18 适配火狐 */
  color: #c4c3cb;
}
input::-moz-placeholder {
  /* Mozilla Firefox 19+ 适配火狐 */
  color: #c4c3cb;
}
input:-ms-input-placeholder {
  /* Internet Explorer 10+  适配ie*/
  color: #c4c3cb;
}
</style>
