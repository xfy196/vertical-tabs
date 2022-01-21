<template>
  <div class="container">
    <div ref="lefNavRef" class="left-nav">
      <div
        ref="navEls"
        @click.stop="handleClick(index)"
        :class="{ 'nav-item': true, active: activeIndex === index }"
        v-for="(value, key, index) in loftData"
        :key="key"
      >
        {{ value.title }}
      </div>
    </div>
    <div @scroll="handleScroll" ref="navContainerRef" class="right-container">
      <div
        class="content"
        ref="contentEls"
        v-for="(value, key) in loftData"
        :key="key"
      >
        <div class="c-item" v-for="(item, index) in 100" :key="index">
          {{ key }} + {{ index }}
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { nanoid } from "nanoid";
import { throttle } from "lodash-es";
export default {
  data() {
    return {
      contentScrollTop: [],
      navScrollTop: [],
      activeIndex: 0,
      loftData: {
        
      },
    };
  },
  created() {
    for (let i = 0; i < 40; i++) {
      this.loftData[`key${i + 1}`] = {
        title: `key${i + 1}`,
      };
      let list = [];
      for (let j = 0; j < 4; j++) {
        list.push({
          id: nanoid(),
          title: `你好-key${i + 1}-${j + 1}`,
        });
      }
      this.loftData[`key${i + 1}`].list = list;
    }
    console.log(this.loftData);
  },
  mounted() {
    let contentEls = this.$refs.contentEls;
    let navEls = this.$refs.navEls;
    for (let i = 0; i < contentEls.length; i++) {
      if (i === 0) {
        this.contentScrollTop.push(0);
        this.navScrollTop.push(0);
      } else {
        this.contentScrollTop.push(
          this.contentScrollTop[i - 1] + contentEls[i].offsetHeight
        );
        this.navScrollTop.push(
          this.navScrollTop[i - 1] + navEls[i].offsetHeight
        );
      }
    }
  },
  watch: {
    activeIndex: {
      handler(val, oldVal) {
        if (val > oldVal) {
          this.$refs.lefNavRef.scrollTo({
            top: this.navScrollTop[val] - this.$refs.navContainerRef.offsetHeight / 2,
            behavior: "smooth",
          });
        } else if (val < oldVal) {
          this.$refs.lefNavRef.scrollTo({
            top: this.$refs.lefNavRef.scrollTop - (this.navScrollTop[oldVal] - this.navScrollTop[val] - this.$refs.navContainerRef.offsetHeight / 2),
            behavior: "smooth",
          });
        }
      },
    },
  },
  methods: {
    handleClick(index) {
      let scrollTop = this.contentScrollTop[index];
      this.$refs.navContainerRef.scrollTo({
        top: scrollTop,
      });
    },
    handleScroll: throttle(function (e) {
      let scrollTop = e.target.scrollTop;
      for (let i = 0; i < this.contentScrollTop.length - 1; i++) {
        if (
          scrollTop >= this.contentScrollTop[i] &&
          scrollTop < this.contentScrollTop[i + 1]
        ) {
          this.activeIndex = i;
          return;
        } else {
          this.activeIndex = i + 1;
        }
      }
    }, 100),
  },
};
</script>
<style lang="scss" scoped>
.container {
  width: 100%;
  height: 200px;
  display: flex;
  .left-nav {
    width: 80px;
    height: 100%;
    overflow: auto;
    background: green;
    .nav-item {
      height: 40px;
      line-height: 40px;
      border: 1px solid #ccc;
      box-sizing: border-box;
      background: #fff;
      &.active {
        color: red;
      }
    }
  }
  .right-container {
    flex: 1;
    overflow: auto;
    height: 100%;
  }
}
</style>
