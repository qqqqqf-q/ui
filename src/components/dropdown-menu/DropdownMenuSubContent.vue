<script setup lang="ts">
import type { DropdownMenuSubContentEmits, DropdownMenuSubContentProps } from 'reka-ui'
import type { HTMLAttributes } from 'vue'
import { reactiveOmit } from '@vueuse/core'
import {
  DropdownMenuSubContent,
  DropdownMenuPortal,
  useForwardPropsEmits,
} from 'reka-ui'
import { menuContentClass, menuAnchoredMotionClass } from '#/lib/menu'
import MenuScrollArea from '../menu-scroll-area/MenuScrollArea.vue'
import { cn } from '#/lib/utils'

const props = defineProps<DropdownMenuSubContentProps & { class?: HTMLAttributes['class'] }>()
const emits = defineEmits<DropdownMenuSubContentEmits>()

const delegatedProps = reactiveOmit(props, 'class')

const forwarded = useForwardPropsEmits(delegatedProps, emits)
</script>

<template>
  <DropdownMenuPortal>
  <DropdownMenuSubContent
    data-slot="dropdown-menu-sub-content"
    v-bind="forwarded"
    :class="cn(menuContentClass, menuAnchoredMotionClass, 'flex min-h-0 flex-col overflow-hidden max-h-(--reka-dropdown-menu-content-available-height)', 'min-w-[8rem] origin-(--reka-dropdown-menu-content-transform-origin)', props.class)"
  >
    <MenuScrollArea>
      <slot />
    </MenuScrollArea>
  </DropdownMenuSubContent>
  </DropdownMenuPortal>
</template>
