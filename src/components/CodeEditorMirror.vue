<template>
    <code-mirror v-model="value" />
</template>

<script>
 import { ref, defineComponent } from 'vue'

 import CodeMirror from 'vue-codemirror6'

 import CompleteIcon from '../static/images/complete.svg?inline'


 export default {
     components: {
         CodeMirror,
         CompleteIcon
     },
     props: {
         isFileLesson: Boolean,
         editorReady: Boolean,
         code: String,
         solution: String,
         cachedCode: Boolean,
         onMounted: Function,
         onCodeChange: Function,
         resetCode: Function,
         expandChallenge: Boolean,
         cyReplaceWithSolution: Function,
         cyClearDefaultCode: Function,
         trackingData: Object
     },
     data: self => {
         return {
             options: {
                 selectOnLineNumbers: false,
                 lineNumbersMinChars: 3,
                 scrollBeyondLastLine: false,
                 automaticLayout: true,
                 minimap: {
                     enabled: false
                 },
                 scrollbar: {
                     alwaysConsumeMouseWheel: false
                 }
             },
             viewSolution: false
         }
     },
     methods: {
         toggleSolution: function () {
             this.viewSolution = !this.viewSolution

             if (this.viewSolution) {
                 countly.trackEvent(countly.events.CODE_VIEW_SOLUTION, this.trackingData)
             }
         }
     },
     computed: {
         editorHeight: function () {
             if (this.expandChallenge) {
                 return undefined
             } else {
                 const lineHeight = 18
                 // In compact view show at least 12 lines, and at most 25 lines.
                 const lines = Math.min(Math.max(this.code.split('\n').length, 12), 25)
                 const height = lines * lineHeight
                 return height
             }
         }
     }
 }
</script>
