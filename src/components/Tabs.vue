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
        key1: {
          title: "key1",
          list: [
            {
              id: nanoid(),
              title: "你好1",
            },
          ],
        },
        key2: {
          title: "key2",
          list: [
            {
              id: nanoid(),
              title: "你好2",
            },
          ],
        },
        key3: {
          title: "key3",
          list: [
            {
              id: nanoid(),
              title: "你好3",
            },
          ],
        },
        key4: {
          title: "key4",
          list: [
            {
              id: nanoid(),
              title: "你好4",
            },
          ],
        },
        key5: {
          title: "key5",
          list: [
            {
              id: nanoid(),
              title: "你好5",
            },
          ],
        },
        key6: {
          title: "key6",
          list: [
            {
              id: nanoid(),
              title: "你好6",
            },
          ],
        },
         key7: {
          title: "key7",
          list: [
            {
              id: nanoid(),
              title: "你好7",
            },
          ],
        },
         key8: {
          title: "key8",
          list: [
            {
              id: nanoid(),
              title: "你好8",
            },
          ],
        },
        key9: {
          title: "key9",
          list: [
            {
              id: nanoid(),
              title: "你好9",
            },
          ],
        },
      },
    };
  },
  mounted() {
    let contentEls = this.$refs.contentEls;
    let navEls = this.$refs.navEls;
    for (let i = 0; i < contentEls.length; i++) {
      if (i === 0) {
        this.contentScrollTop.push(0);
        this.navScrollTop.push(navEls[i].clientHeight);
      } else {
        this.contentScrollTop.push(
          this.contentScrollTop[i - 1] + contentEls[i].clientHeight
        );
        this.navScrollTop.push(
          this.navScrollTop[i - 1] + navEls[i].clientHeight
        );
      }
    }
  },
  watch: {
    activeIndex: {
      handler(val, oldVal) {
        if (val > oldVal) {
          if (
            this.navScrollTop[val] + this.$refs.navEls[val].clientHeight >=
            this.$refs.lefNavRef.clientHeight
          ) {
            this.$refs.lefNavRef.scrollTo({
              top: this.navScrollTop[val],
              behavior: "smooth",
            });
          }
        } else {
          this.$refs.lefNavRef.scrollTo({
            top: 0,
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
