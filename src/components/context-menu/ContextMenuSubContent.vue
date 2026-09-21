<script setup lang="ts">
import type { ContextMenuSubContentEmits, ContextMenuSubContentProps } from 'reka-ui'
import type { HTMLAttributes } from 'vue'
import { reactiveOmit } from '@vueuse/core'
import {
  ContextMenuSubContent,
  ContextMenuPortal,
  useForwardPropsEmits,
} from 'reka-ui'
import { menuContentClass, menuSlideClass } from '#/lib/menu'
import MenuScrollArea from '../menu-scroll-area/MenuScrollArea.vue'
import { cn } from '#/lib/utils'

const props = defineProps<ContextMenuSubContentProps & { class?: HTMLAttributes['class'] }>()
const emits = defineEmits<ContextMenuSubContentEmits>()

const delegatedProps = reactiveOmit(props, 'class')

const forwarded = useForwardPropsEmits(delegatedProps, emits)
</script>

<template>
<ContextMenuPortal>
  <ContextMenuSubContent
    data-slot="context-menu-sub-content"
    v-bind="forwarded"
    :class="
      cn(
        menuContentClass,
        menuSlideClass,
        'flex min-h-0 flex-col overflow-hidden max-h-(--reka-context-menu-content-available-height)',
        'min-w-[8rem] origin-(--reka-context-menu-content-transform-origin)',
        props.class,
      )
    "
  >
    <MenuScrollArea><slot /></MenuScrollArea>
  </ContextMenuSubContent>
</ContextMenuPortal>
</template>
