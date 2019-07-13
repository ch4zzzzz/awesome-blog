<template>
  <div id="container">
    <BlogHeader id="blog-header" class="blog-header" 
        :class="{
          'header-pinned': !isOnTop && !isHeaderOnScrollingDown,
          'header-unpinned': !isOnTop && isHeaderOnScrollingDown
        }"/>
    <nuxt :style="{'margin-top': headerHight}"/>
  </div>
</template>

<script lang="ts">
import Component from "vue-class-component";
import Vue from 'vue';

@Component({
  components: {
    BlogHeader: () => import('~/components/BlogHeader.vue')
  }
})
export default class Default extends Vue {
  
  // data
  scrollY: number = 0;
  isOnTop: boolean = true;
  isHeaderOnScrollingDown: boolean = false;
  headerHight: string = '0px';

  created () {
    if (process.browser) {
      window.addEventListener('scroll', this.onScroll);
    }
  }

  mounted () {
    const header = document.querySelector('#blog-header');
    console.log(typeof header);
    const height: string = getComputedStyle(header).getPropertyValue('height');
    this.headerHight = height;
    if (height) {

    }
  }

  destroy () {
    if (process.browser) {
      window.removeEventListener('scroll', this.onScroll);
    }
  }
  
  // methods
  onScroll (event) {
    const scrollY = window.scrollY;
    const lastScrollY = this.scrollY;
    this.scrollY = scrollY;
    if (scrollY === 0) {
      this.isOnTop = true;
      this.isHeaderOnScrollingDown = false;
      return;
    } else {
      this.isOnTop = false;
    }
    if (scrollY - lastScrollY < 0) {
      window.requestAnimationFrame(() => {
        this.isHeaderOnScrollingDown = false;
      })
    } else {
      this.isHeaderOnScrollingDown = true;
    }
  }
}
</script>

<style>
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
}

/* #container {
  
} */

.blog-header {
  top: 0px;
  transition: all 0.5s ease-out;
  /* position: sticky; */
  position: absolute;
  width: 100%;
}

.header-unpinned {
  top: -200px;
  /* position: sticky; */
  /* position: fixed; */
}

.header-pinned {
  top: 0px;
  /* width: 100%; */
  position: fixed;
}
</style>
