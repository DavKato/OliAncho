<template>
  <div class="walls" :class="currentRoute">
    <div class="wall" :class="`${currentRoute}__wall`"></div>
    <div class="wall" :class="`${currentRoute}__wall`"></div>
    <div class="wall" :class="`${currentRoute}__wall`"></div>
    <div class="wall" :class="`${currentRoute}__wall`"></div>
  </div>
</template>

<style lang="scss" scoped>
.walls {
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: space-between;
}

$commonW: 24%;
@mixin wallsborder {
  box-shadow: inset 0 0.8rem 0 $gray-d;
}

.index {
  &__wall {
    width: $commonW;
    opacity: 1;
    &:not(:last-child) {
      border-right: $bdw-p solid $gray-d;
      margin-left: $bdw-p + 0.2rem;
    }
  }
}
.about {
  @include wallsborder();
  opacity: 1;
  &__wall {
    width: $commonW;
    &:first-child {
      background-color: $white-p;
    }
  }
}
.works {
  @include wallsborder();
  opacity: 1;
  &__wall {
    width: $commonW;
    &:nth-child(2) {
      background-color: $white-p;
    }
  }
}
.blog {
  @include wallsborder();
  opacity: 1;
  &__wall {
    width: $commonW;
    &:nth-child(3) {
      background-color: $white-p;
    }
  }
}
.location {
  @include wallsborder();
  opacity: 1;
  &__wall {
    width: $commonW;
    &:nth-child(4) {
      background-color: $white-p;
    }
  }
}
</style>

<script>
export default {
  computed: {
    currentRoute() {
      return this.$route.name.split("___")[0];
    }
  }
};
</script>
