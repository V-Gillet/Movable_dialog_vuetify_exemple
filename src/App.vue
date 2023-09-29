<template>
  <v-app>
    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
import HelloWorld from '@/components/HelloWorld.vue'
import { onMounted } from 'vue'

interface IMovableDialog{
  el?: HTMLElement,
  mouseStartX: number,
  mouseStartY: number,
  elStartX: number,
  elStartY: number,
  oldTransition: string
}
//  Make all application dialogs movable. Base solution by tipsy found in the Vuetify issue : https://github.com/vuetifyjs/vuetify/issues/4058
onMounted(
  () => {
    const dialog: IMovableDialog = {
      mouseStartX: 0,
      mouseStartY: 0,
      elStartX: 0,
      elStartY: 0,
      oldTransition: ''
    }
    document.addEventListener('mousedown', e => {
      const closestDialog: HTMLElement | null = (e.target as HTMLElement).closest('.v-overlay__content')
      console.log(closestDialog)
      if (e.button === 0 && closestDialog != null && (e.target as HTMLElement).classList.contains('v-card-title')) { // element which can be used to move element
        dialog.el = closestDialog // element which should be moved
        if (dialog.el !== undefined) {
          dialog.mouseStartX = e.clientX
          dialog.mouseStartY = e.clientY
          dialog.elStartX = dialog.el.getBoundingClientRect().left
          dialog.elStartY = dialog.el.getBoundingClientRect().top
          dialog.el.style.position = 'fixed'
          dialog.el.style.margin = '0'
          dialog.oldTransition = dialog.el.style.transition
          dialog.el.style.transition = 'none'
        }
      }
    })
    document.addEventListener('mousemove', e => {
      const closestDialog: HTMLElement | null = (e.target as HTMLElement).closest('.v-overlay__content')
      if (dialog.el === undefined) return
      if (dialog.el === closestDialog) {
        // eslint-disable-next-line
        document.getSelection()?.empty()
      }
      dialog.el.style.left = Math.min(
        Math.max(dialog.elStartX + e.clientX - dialog.mouseStartX, 0),
        window.innerWidth - dialog.el.getBoundingClientRect().width
      ) + 'px'
      dialog.el.style.top = Math.min(
        Math.max(dialog.elStartY + e.clientY - dialog.mouseStartY, 0),
        window.innerHeight - dialog.el.getBoundingClientRect().height
      ) + 'px'
    })
    document.addEventListener('mouseup', () => {
      if (dialog.el === undefined) return
      dialog.el.style.transition = dialog.oldTransition
      dialog.el = undefined
    })
    setInterval(() => { // prevent out of bounds
      const dialog: HTMLElement | null = document.querySelector('.v-overlay__content')
      if (dialog === null) return
      dialog.style.left = Math.min(parseInt(dialog.style.left), window.innerWidth - dialog.getBoundingClientRect().width) + 'px'
      dialog.style.top = Math.min(parseInt(dialog.style.top), window.innerHeight - dialog.getBoundingClientRect().height) + 'px'
    }, 2000)
  }
)
</script>

<style lang="css">
.v-dialog > .v-overlay__content {
  cursor: move;
}
</style>
