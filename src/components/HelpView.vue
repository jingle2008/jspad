<template>
  <nav class="panel">
    <p class="panel-heading has-text-weight-semibold">
      Keyboard Shortcuts
    </p>
    <div class="panel-block">
      <p class="control">
        <b-input placeholder="Search..." type="search"
          icon="magnify" />
      </p>
    </div>
    <p class="panel-tabs">
      <a class="is-active">all</a>
      <a>selection</a>
      <a>edit</a>
      <a>navigation</a>
      <a>search</a>
    </p>
    <div
      v-for="command in commands"
      :key="command.name"
      class="panel-block">
      <div>
        <div class="is-pulled-left">
          <p class="title is-6">
            {{ command.name }}
          </p>
        </div>
        <div class="is-pulled-right">
          <span
            v-if="command.shortcutWin"
            class="hotkey">
            {{ command.shortcutWin }}
          </span>
          <span
            v-if="command.shortcutWin && command.shortcutMac">
            /
          </span>
          <span
            v-if="command.shortcutMac"
            class="hotkey">
            {{ command.shortcutMac }}
          </span>
        </div>
      </div>
      <div class="is-clearfix">
        <p class="subtitle is-6">
          {{ command.description }}
        </p>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'HelpView',
  data() {
    return {
      commands: [
        {
          name: 'selectAll',
          description: 'Select the whole content of the editor.',
          shortcutWin: 'Ctrl-A',
          shortcutMac: 'Cmd-A',
          type: 'selection',
        },
        {
          name: 'delWrappedLineLeft',
          description: 'Delete the part of the line from the left side of the visual line the cursor is on to the cursor.',
          shortcutWin: null,
          shortcutMac: 'Cmd-Backspace',
          type: 'edit',
        },
      ],
    };
  },
};
</script>

<style scoped>
.panel-block {
  flex-direction: column;
  align-items: stretch;
}
.hotkey {
  display: inline-block;
  color: #fff;
  background-color: #333;
  border: 1px solid #333;
  border-radius: 5px;
  text-align: center;
  margin-right: 5px;
  box-shadow: inset 0 1px 0 #666, 0 1px 0 #bbb;
  padding: 5px 9px;
  font-size: 0.75em;
}
</style>
