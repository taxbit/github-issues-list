<template>
  <div class="list__item">
    <div class="list__item-issue">
      {{issue.title}}
      <div class="list__item-labels" >
        <template v-for="label in issue.labels">
          <div :key="label.id" class="list__item-label" :style="{backgroundColor: '#' + label.color}">
            <span>
              {{ label.name }}
            </span>
          </div>
        </template>
      </div>
    </div>
    <div v-if="issue.comments" class="list__item-comment">
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

<style lang="scss">

$small: 320px;
$large: 1024px;

@mixin respond-to($media) {
  @if $media == handhelds {
    @media only screen and (max-width: $small) { @content; }
  }
  @else if $media == medium-screens {
    @media only screen and (min-width: $small + 1) and (max-width: $large - 1) { @content; }
  }
  @else if $media == wide-screens {
    @media only screen and (min-width: $large) { @content; }
  }
}


.list__item {
  display: flex;
  margin: 5px;
  align-items: center;
  justify-content: space-between;
  text-align: left;

  &:hover {
    border-left: 2px solid green;
    margin-left: 3px;
  }

  &:not(:last-of-type) {
    border-bottom: 1px solid #dedede;
    padding-bottom: 5px;
  }

  &-issue {
    font-size: 14px;
    padding: 1px 4px;
  }

  &-labels {
    display: flex;
    flex-wrap: wrap;
    @include respond-to(handhelds) {
        flex-direction: column ;
        align-items: flex-start;
      }
  }

  &-label {
    font-size: 12px;
    padding: 2px 5px;
    color: white;
    border-radius: 3px;

    span {
      color: white;
    }

    &:not(:last-of-type) {
      margin-right: 5px;
      @include respond-to(handhelds) {
        margin-bottom: 3px;
      }
    }
  }

  &-comment {
    font-size: 10px;
    border: 1px solid #9c9c9c;
    padding: 2px 6px;
    border-radius: 16px;
    border-bottom-left-radius: 0;
  }

}

</style>
