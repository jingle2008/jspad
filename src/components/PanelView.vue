<template>
  <div class="panel-container">
    <p class="panel-heading has-text-weight-semibold">
      <b-icon :icon="icon" type="is-primary">
      </b-icon>
      {{ title }}
    </p>
    <div class="panel-content">
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  name: 'PanelView',
  props: {
    title: {
      type: String,
      required: true,
    },
    icon: {
      type: String,
      required: true,
    },
  },
};
</script>

<style>
.panel-container {
  display: flex;
  flex-direction: column;
}
.panel-content {
  flex: 1;
  overflow: hidden;
  border-left: 1px solid #dbdbdb;
  border-right: 1px solid #dbdbdb;
  border-bottom: 1px solid #dbdbdb;
}
</style>
