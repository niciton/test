<template>
  <div
			v-bind="$attrs"
			class="wrapper"
			:style="[{paddingBottom: footerHeight + 'px'}, {paddingTop: headerHeight + 'px'}]">
    <app-header @size="headerHeight = $event"></app-header>
    <menu-all></menu-all>
    <div class="help-bar container">
      <ol class="breadcrumbs" v-if="$route.path != '/'">
        <li class="breadcrumbs_item">
          <router-link class="breadcrumbs_link" to="/">
        <span class="text">
          Главная
        </span>
          </router-link>
        </li>
        <li class="breadcrumbs_item">
        <span class="breadcrumbs_item__text">
          {{ $route.meta.name }}
        </span>
        </li>
      </ol>
      <h1>
        {{ $route.meta.title || $route.meta.name }}
      </h1>
    </div>
    <main class="main">
      <slot></slot>
    </main>
    <app-footer @size="footerHeight = $event"></app-footer>
    <go-top></go-top>
  </div>
</template>

<script>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import '@/css/main.scss';
import AppHeader from '@/components/Header.vue';
import AppFooter from '@/components/Footer.vue';
import MenuAll from '@/components/MenuAll.vue';
import GoTop from "@components/GoTop.vue";

import {mapState, mapMutations} from 'vuex';

//:style="{paddingBottom: $refs.footer.clientHeight+ 'px'}"
export default {
  // name: 'App',
  data() {
    return {
      footerHeight: 55,
			headerHeight: 150,
    }
  },
  methods: {
    ...mapState({
      menuClass: state => state.menuStore.menuClass
    }),
    ...mapMutations({
      toggleMenuClass: 'menuStore/toggleMenuClass',
      closeMenu: 'menuStore/closeMenu',
			closePopups: 'popups/closePopups',
    }),
    toggleMenu(className) {
      this.closeMenu();
      this.toggleMenuClass(className);
    }
  },
  components: {
    AppHeader,
    MenuAll,
    AppFooter,
    GoTop,
  },
  mounted() {
    document.addEventListener('click', (e) => {
      this.closeMenu();
    })
    document.addEventListener('keydown', (e) => {
      if (e.which === 27) {
				this.closeMenu();
				this.closePopups();
			}
    });
  },
  // components: { DefaultBtn },
}
</script>

<style>

</style>