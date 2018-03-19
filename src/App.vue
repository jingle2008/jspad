<template>
  <div id="app">
    <section class="app-layout">
      <div class="app-head">
        <header class="navbar is-primary">
          <div class="container is-fluid">
            <div class="navbar-brand">
              <a class="navbar-item">
                <b-icon icon="flask" size="is-medium"
                  type="is-light">
                </b-icon>
                <span class="is-size-4 ml-1">
                  JS Playground
                </span>
              </a>
              <span class="navbar-burger burger"
                :class="{ 'is-active': menuOpen}"
                @click.stop="menuOpen = !menuOpen">
                <span></span>
                <span></span>
                <span></span>
              </span>
            </div>
            <div class="navbar-menu"
              :class="{ 'is-active': menuOpen }">
              <div class="navbar-end">
                <div class="navbar-item">
                  <a href="#" target="_blank">
                    <b-icon icon="github-circle"
                      size="is-medium"
                      :type="menuOpen ? 'is-primary' : 'is-light'">
                    </b-icon>
                  </a>
                </div>
              </div>
            </div>
          </div>
        </header>
      </div>
      <div class="app-body">
        <playground />
      </div>
    </section>
  </div>
</template>

<script>
import Playground from './components/Playground.vue';

export default {
  name: 'app',
  data() {
    return {
      menuOpen: false,
    };
  },
  components: {
    Playground,
  },
  methods: {
  },
};
</script>

<style>
body {
  min-height: 100vh;
}
.app-layout {
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  position: absolute;
  display: flex;
  flex-direction: column;
}
.app-body {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.mt-1 {
  margin-top: 1rem;
}
.ml-1 {
  margin-left: 1rem;
}
.mr-1 {
  margin-right: 1rem;
}
.mb-1 {
  margin-bottom: 1rem;
}
</style>
