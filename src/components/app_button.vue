<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  color: { default: '#34495E' },
  clickable: { default: false },
  border: { default: '#34495E' },
  background: { default: undefined },
  dropdown: { default: false },
  height: { default: '55px' },
});

const background = computed(() => props.background ?? props.border);
const hovering_card = ref(false);
const hovering_dropdown = ref(false);
</script>

<template lang="pug">
.card__container(@mouseover="hovering_card = true" @mouseleave="hovering_card = false")
  .card
    .content__container
      .shadow(v-if="props.clickable")
      .content(:class="{ clickable: props.clickable }")
        slot(name="content")
        fa.arrow(:icon="['fas', 'angle-down']" v-if="props.dropdown")
  .drop_down__container(v-if="props.dropdown && (hovering_card || hovering_dropdown)" @mouseover="hovering_dropdown = true" @mouseleave="hovering_dropdown = false")
    .drop_down
      slot(name="dropdown")
</template>

<style lang="stylus" scoped>
.card__container
  position relative
  user-select none
  .card
    cursor pointer
    padding 4px 0 0 4px
    width max-content
    .content__container
      position relative
      .content
        border-radius 12px
        height 55px
        position relative
        border 1px solid #34495E
        background linear-gradient(to right, #e0eafc, #cfdef3)
        display flex
        justify-content center
        align-items center
        .arrow
          color v-bind('props.color')
          margin-right .5em
        &.clickable
          left -4px
          top -4px
          transition left .1s ease, top .1s ease
          &:hover
            left 0
            top 0
      .shadow
        border-radius 12px
        position absolute
        top 0
        left 0
        width 100%
        height 100%
        border 1px solid #34495E
        background linear-gradient(to right, #7f7fd5, #86a8e7, #91eae4)

  .drop_down__container
    position absolute
    color v-bind('props.color')
    right 0
    padding-top .5em
    width 100%
    z-index 10
    .drop_down
      border 1px solid v-bind('color')
      background #eee
      border-radius 12px
      min-width max-content
      padding .5em
</style>
