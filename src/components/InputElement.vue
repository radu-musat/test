<template>
  <div class="input-element">
    <i :class="props.icon"></i>
    <input :type="passwordType" :placeholder="props.placeholder" />
    <button
      v-if="props.type === 'password'"
      @click.prevent="passwordIsVisible = !passwordIsVisible"
      :class="{ visible: passwordIsVisible }"
    >
      <i class="toggle"></i>
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import type { ComputedRef } from 'vue'
interface Props {
  type: string
  placeholder: string
  icon: 'default' | 'alternate'
}
const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'Your name',
  icon: 'default'
})

const passwordIsVisible = ref(false)
const passwordType: ComputedRef<string> = computed((): string => {
  return props.type !== 'password'
    ? props.type
    : props.type === 'password' && passwordIsVisible.value
      ? 'text'
      : 'password'
})
</script>

<style scoped lang="scss">
.input-element {
  align-items: center;
  border: 2px solid $gray-2;
  border-radius: 45px;
  display: flex;
  flex-grow: 1;
  height: 4.7rem;
  padding: 1rem;

  button {
    border: 0;
    background: none;
    cursor: pointer;
    padding: 5px;
    position: relative;

    &::before {
      background-color: $dark-1;
      content: '';
      display: block;
      height: 2px;
      left: 0;
      position: absolute;
      top: 50%;
      width: 100%;
      transform: translateY(-50%) rotate(45deg);
    }

    &.visible::before {
      display: none;
    }
  }

  i {
    display: block;
    height: 1.5rem;
    margin-right: 1.1rem;
    width: 1.5rem;

    &.default {
      background: url('@/assets/icons/generic_icon.svg') center/cover no-repeat;
    }

    &.alternate {
      background: url('@/assets/icons/circle.svg') center/cover no-repeat;
    }

    &.toggle {
      background: url('@/assets/icons/circle-dot.svg') center/cover no-repeat;
      margin: 0;
    }
  }

  input {
    background: transparent;
    border: 0;
    border-left: 4px solid $gray-2;
    color: $dark-1;
    display: block;
    flex-grow: 1;
    font-size: 1rem;
    padding: 0.9rem 1.6rem;
    position: relative;

    &:focus {
      outline: none;
    }

    &::placeholder {
      font-size: 1rem;
    }
  }
}
</style>
