<script>
import Vue from 'vue';
import Store from '../stores/repo_store';
import RepoTab from './repo_tab.vue';
import RepoMixin from '../mixins/repo_mixin';

const RepoTabs = {
  mixins: [RepoMixin],

  components: {
    'repo-tab': RepoTab,
  },

  data: () => Store,

  methods: {
    isOverflow() {
      return this.$el.scrollWidth > this.$el.offsetWidth;
    },

    xClicked(file) {
      Store.removeFromOpenedFiles(file);
    },
  },

  watch: {
    openedFiles() {
      Vue.nextTick(() => {
        this.tabsOverflow = this.isOverflow();
      });
    },
  },
};

export default RepoTabs;
</script>

<template>
<ul id="tabs" v-if="isMini" v-cloak :class="{'overflown': tabsOverflow}">
  <repo-tab v-for="tab in openedFiles" :key="tab.id" :tab="tab" :class="{'active' : tab.active}" @xclicked="xClicked"/>
  <li class="tabs-divider" />
</ul>
</template>
