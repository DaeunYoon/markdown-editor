<script setup lang="ts">
import { Codemirror } from 'vue-codemirror'
import { markdown, markdownLanguage } from '@codemirror/lang-markdown'
import { marked } from 'marked'
// import { javascript } from '@codemirror/lang-javascript'
// import { oneDark } from '@codemirror/theme-one-dark'

const code = ref('')
// const extensions = [javascript(), oneDark]

// Codemirror EditorView instance ref
const view = shallowRef()

// Status is available at all times via Codemirror EditorView
const getCodemirrorStates = () => {
  const state = view.value.state
  const ranges = state.selection.ranges
  const selected = ranges.reduce((r, range) => r + range.to - range.from, 0)
  const cursor = ranges[0].anchor
  const length = state.doc.length
  const lines = state.doc.lines
  // more state info ...
  // return ...
}

const markdownToHtml = computed(() => {
  console.log(marked(code.value))
  return marked(code.value)
})

function log(str: string) {
  console.log(str)
}
</script>

<template>
  <div class="flex h-full">
    <Codemirror
      v-model="code"
      :style="{ height: '100%', width: '50%' }"
      :autofocus="true"
      :indent-with-tab="true"
      :tab-size="2"
      :extensions=" markdown({ base: markdownLanguage })"
      @change="log(`change ${$event}`)"
    />
    <div class="w-half" v-html="markdownToHtml" />
  </div>
</template>
