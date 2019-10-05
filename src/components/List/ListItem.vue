<template>
  <div class="list__item">
        <div class="list__item-title">
          {{issue.title}}
        </div>
        <template v-for="label in issue.labels">
          <div class="list__item-label" :key="label.id" :style="{backgroundColor: '#' + label.color}">{{ label.name }}</div>
        </template>
        <div class="list__item-comment">
          {{issue.comments}}
        </div>
  </div>
</template>

<script>
  export default {
    name: 'ListItem',
    props: {
      issue: {
        type: Object,
        default: {}
      },
    },
    data() {
      return {
      }
    },
  }
</script>

<style lang="scss" scoped>
.list__item {
  display: flex;
  margin: 5px;

  &-title {

  }

}
</style>
