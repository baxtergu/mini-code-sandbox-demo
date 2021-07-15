<template>
  <div class="app-wrapper">
    <div class="editor-wrapper">
      <template v-for="item in Object.values(codeMap)" v-bind:key="item.path">
        <div class="file-name">{{ item.path }}</div>
        <textarea
          class="code-editor"
          @change="noticeSandboxUpdate"
          v-model="codeMap[item.path].code"
        />
      </template>
    </div>
    <div class="sandbox-wrapper">
      <iframe
        id="sandbox"
        @load="noticeSandboxUpdate"
        src="/sandbox.html"
        frameborder="0"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      codeMap: {
        "/src/index.js": {
          code: `
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);`.trim(),
          path: "/src/index.js",
        },
        "/src/App.jsx": {
          code: `
import React, { useState } from 'react'
import { title } from './data.json'
import './App.css'

export default function App() {
  const [count, setCount] = useState(0)
  return (
    <div className="App">
      <header className="App-header">
        <p>Hello {title}!</p>
        <p>
          <button onClick={() => setCount((count) => count + 1)}>
            count is: {count}
          </button>
        </p>
        <p>
          Edit <code>App.jsx</code> and save to test HMR updates.
        </p>
        <p>
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </p>
      </header>
    </div>
  )
}
`.trim(),
          path: "/src/App.jsx",
          style: {
            flex: 1,
          },
        },
        "/src/data.json": {
          code: `{ "title": "Mini Sandbox - Json Data" }`,
          path: "/src/data.json",
        },
        "/src/App.css": {
          code: `
body {
  padding: 0;
  margin: 0;
}
.App {
  text-align: center;
}

.App-header {
  background-color: #282c34;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
  color: white;
}

.App-link {
  color: #61dafb;
}

button {
  font-size: calc(10px + 2vmin);
}
`.trim(),
          path: `/src/App.css`,
        },
      },
    };
  },
  methods: {
    noticeSandboxUpdate() {
      document.querySelector("#sandbox").contentWindow.postMessage({
        codeMap: JSON.parse(JSON.stringify(this.codeMap)),
        entry: "/src/index.js",
        dependencies: {},
        externals: {
          react: "React",
          "react-dom": "ReactDOM",
        },
      });
    },
  },
};
</script>

<style>
.app-wrapper {
  width: 100vw;
  height: 100vh;
  display: flex;
  flex-direction: row;
}
.app-wrapper > .editor-wrapper {
  flex: 1;
  display: flex;
  flex-direction: column;
  width: 100%;
  height: 100%;
}

.app-wrapper > .editor-wrapper > .file-name {
  font-size: 24px;
  font-weight: bold;
  width: 100%;
  line-height: 30px;
}

.app-wrapper > .editor-wrapper > .code-editor {
  flex: 1;
  width: 100%;
  line-height: 30px;
}

.app-wrapper > .sandbox-wrapper {
  flex: 2;
  width: 100%;
  height: 100%;
}

.app-wrapper > .sandbox-wrapper > iframe {
  width: 100%;
  height: 100%;
}
</style>
