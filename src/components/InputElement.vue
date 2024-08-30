<template>
  <div class="input-element-container">
    <div
      class="input-element"
      :class="{ tel: props.type === 'tel' }"
      :data-prefix="props.telPrefix"
    >
      <i :class="props.icon"></i>
      <input
        :type="passwordType"
        :placeholder="props.placeholder"
        autocomplete="true"
        v-model="model"
      />
      <button
        v-if="props.type === 'password'"
        @click.prevent="$emit('togglePasswordVisibility')"
        :class="{ visible: passwordIsVisible }"
      >
        <i class="toggle"></i>
      </button>
    </div>
    <small class="input-errors" v-for="error of errors" :key="error.$uid">
      <div class="error-msg">{{ error.$message }}</div>
    </small>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import type { ComputedRef } from 'vue'
interface Props {
  type?: string
  placeholder?: string
  icon?: 'default' | 'alternate'
  passwordIsVisible?: boolean
  errors?: any[]
  telPrefix?: string
}

const model = defineModel()
const props = withDefaults(defineProps<Props>(), {
  type: 'text',
  placeholder: 'Your name',
  icon: 'default',
  passwordIsVisible: false,
  telPrefix: '60'
})

const emit = defineEmits(['togglePasswordVisibility'])

const passwordType: ComputedRef<string> = computed((): string => {
  return props.type !== 'password'
    ? props.type
    : props.type === 'password' && props.passwordIsVisible
      ? 'text'
      : 'password'
})
</script>

<style scoped lang="scss">
.input-element-container {
  flex-grow: 1;
}

.input-element {
  align-items: center;
  border: 2px solid $gray-2;
  border-radius: 45px;
  display: flex;
  height: 4.7rem;
  padding: 1rem;
  position: relative;

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
.input-errors {
  color: $red-1;
  display: block;
  font-size: 10px;
  padding-top: 5px;
  text-align: center;

  @media screen and (min-width: $breakpoint-md) {
    font-size: 14px;
    text-align: left;
    padding: 5px 0 0 3.5rem;
  }
}

.tel::before {
  content: attr(data-prefix);
  display: block;
  position: absolute;
  left: 4.2rem;
  top: 50%;
  transform: translateY(-60%);
}
</style>
