<template>
  <div class="playground">
    <div class="split split-horizontal help-panel">
      <help-view />
    </div>
    <div class="split split-horizontal main-panel">
      <div class="toolbar">
        <b-field>
          <p class="control">
            <button class="button" @click="compile">
              <b-icon icon="format-bold"></b-icon>
            </button>
          </p>
          <p class="control">
            <button class="button">
              <b-icon icon="format-italic"></b-icon>
            </button>
          </p>
          <p class="control">
            <button class="button">
              <b-icon icon="format-underline"></b-icon>
            </button>
          </p>
          <p class="control">
            <button class="button">
              <b-icon icon="format-align-left"></b-icon>
            </button>
          </p>
          <p class="control">
            <button class="button">
              <b-icon icon="format-align-center"></b-icon>
            </button>
          </p>
          <p class="control">
            <button class="button">
              <b-icon icon="format-align-right"></b-icon>
            </button>
          </p>
          <b-input placeholder="Search..." type="search"
            icon="magnify"></b-input>
        </b-field>
      </div>
      <div class="split-panel">
        <div class="split editor">
          <panel-view
            title="Javascript"
            icon="script"
            class="split split-horizontal source">
            <textarea ref="source"></textarea>
          </panel-view>
          <panel-view
            title="Transpiled"
            icon="transfer"
            class="split split-horizontal babel">
            <textarea ref="transpiled"></textarea>
          </panel-view>
        </div>
        <panel-view
          title="Console Output"
          icon="calculator"
          class="split output">
          <div class="results"></div>
        </panel-view>
      </div>
    </div>
  </div>
</template>

<script>
import Split from 'split.js';
import * as Babel from '@babel/standalone';
import CodeMirror from 'codemirror';
import 'codemirror/lib/codemirror.css';
import 'codemirror/mode/javascript/javascript';
import 'codemirror/addon/edit/matchbrackets';
import 'codemirror/addon/edit/closebrackets';
import 'codemirror/addon/dialog/dialog';
import 'codemirror/addon/dialog/dialog.css';
import 'codemirror/addon/search/searchcursor';
import 'codemirror/addon/search/search';
import 'codemirror/addon/selection/active-line';
import 'codemirror/addon/search/matchesonscrollbar';
import 'codemirror/addon/search/matchesonscrollbar.css';
import 'codemirror/addon/fold/foldgutter.css';
import 'codemirror/addon/fold/foldgutter';
import 'codemirror/addon/fold/foldcode';
import 'codemirror/addon/fold/brace-fold';
import 'codemirror/addon/scroll/annotatescrollbar';

import PanelView from './PanelView.vue';
import HelpView from './HelpView.vue';

function tabWithSpace(cm) {
  if (cm.somethingSelected()) {
    cm.indentSelection('add');
  } else {
    cm.execCommand('insertSoftTab');
  }
}

export default {
  name: 'Playground',
  props: {
    msg: String,
  },
  data() {
    return {
      cmOptions: {
        mode: 'text/javascript',
        tabSize: 2,
        lineNumbers: true,
        lineWrapping: true,
        matchBrackets: true,
        autoCloseBrackets: true,
        styleActiveLine: true,
        showMatchesOnScrollbar: true,
        foldGutter: true,
        gutters: [
          'CodeMirror-linenumbers',
          'CodeMirror-foldgutter',
        ],
        extraKeys: {
          Tab: tabWithSpace,
          'Shift-Tab': cm => cm.indentSelection('subtract'),
        },
      },
      sourceEditor: null,
      targetEditor: null,
    };
  },
  methods: {
    compile() {
      const source = this.sourceEditor.getValue();
      const output = Babel.transform(
        source,
        { presets: ['es2015'] },
      ).code;

      this.targetEditor.setValue(output);
    },
  },
  components: {
    PanelView,
    HelpView,
  },
  mounted() {
    Split(['.help-panel', '.main-panel'], {
      sizes: [20, 80],
      minSize: [250, 400],
    });

    Split(['.source', '.babel'], {
      sizes: [50, 50],
    });

    Split(['.editor', '.output'], {
      sizes: [60, 40],
      minSize: 200,
      direction: 'vertical',
    });

    this.sourceEditor = CodeMirror.fromTextArea(
      this.$refs.source,
      this.cmOptions,
    );

    this.targetEditor = CodeMirror.fromTextArea(
      this.$refs.transpiled,
      this.cmOptions,
    );
  },
};
</script>

<style>
.playground {
  flex: 1;
  display: flex;
  flex-direction: row;
}
.main-panel {
  display: flex;
  flex-direction: column;
}
.split-panel {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.CodeMirror, .results {
  height: 100%;
  width: 100%;
}
.gutter {
  background-color: #eee;
  background-repeat: no-repeat;
  background-position: 50%;
}
.gutter.gutter-horizontal {
  cursor: col-resize;
  background-image: url('../assets/vertical.png');
}
.gutter.gutter-vertical {
  cursor: row-resize;
  background-image: url('../assets/horizontal.png');
}
.split.split-horizontal, .gutter.gutter-horizontal {
  height: 100%;
  float: left;
}
.split {
  overflow-y: auto;
  overflow-x: hidden;
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
  font-size: 1em;
}
</style>
