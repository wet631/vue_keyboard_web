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
      <div class="inputMain" @click="inputMainBtn">
        <input
          type="text"
          placeholder="请输入车牌"
          v-model="inputNumber"
          :disabled="showInput"
          class="vnp_input"
        />
      </div>
      <div class="iconBox">
        <img
          src="./pic/danchujianpan.png"
          alt=""
          :class="rotateA ? 'picBox' : 'picBox go'"
          @click="showBtn"
        />
        <transition name="fade">
          <vnp_keyboard
            v-show="showKeyBoard"
            @carName="carName"
            class="vnp_keyBoard"
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
  methods: {
    showBtn() {
      // const inputElement = document.getElementsByClassName("inputMain");
      // const floatingDivElement =
      //   document.getElementsByClassName("vnp_keyBoard");
      // // 获取输入框相对于文档顶部的距离和高度
      // const inputTop =
      //   inputElement.getBoundingClientRect().top + window.pageYOffset;
      // const inputHeight = inputElement.offsetHeight;

      // // 获取当前窗口高度
      // const windowHeight = window.innerHeight;

      // // 判断是否需要在上方展示浮动 `div`
      // if (
      //   inputTop + inputHeight + floatingDivElement.offsetHeight >
      //   windowHeight
      // ) {
      //   // 在输入框上方展示
      //   floatingDivElement.style.top = `${
      //     inputTop - floatingDivElement.offsetHeight
      //   }px`;
      // } else {
      //   // 在输入框下方展示
      //   floatingDivElement.style.top = `${inputTop + inputHeight}px`;
      // }
      this.showKeyBoard = !this.showKeyBoard;
      // this.showInput = true;
      this.rotateA = !this.rotateA;
    },
    inputMainBtn() {},
    carName(val) {
      this.inputNumber = val;
      this.showKeyBoard = false;
      this.showInput = false;
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
