<template>
  <div class="container">
    <form class="form">
      <template v-if="currentStep === 1">
        <div class="input-group-container">
          <input-element />
          <input-element placeholder="Your Surname" />
        </div>

        <div class="input-group-container">
          <input-element placeholder="Your Username" />
          <input-element placeholder="E-mail" type="email" />
        </div>

        <div class="input-group-container">
          <input-element placeholder="Your Password" type="password" />
          <input-element placeholder="Reenter password" type="password" />
        </div>
      </template>
      <template v-else>
        <div class="input-group-container">
          <input-element type="number" placeholder="Identification Number" />
          <input-element type="tel" placeholder="Telephone" />
        </div>
        <div class="input-group-container">
          <input-element placeholder="Security Question" />
          <input-element placeholder="Your Answer" />
        </div>

        <div class="radios">
          <h3 class="radios-title">LOGIN SETUP MENU</h3>
          <div class="radios-input-group">
            <label class="radios-input-group__input">
              <input type="radio" id="login" checked name="setup" value="login" />
              <span class="checkmark"></span>
              <span class="text">Active login email</span>
            </label>

            <label class="radios-input-group__input">
              <input type="radio" id="sms" name="setup" value="sms" />
              <span class="checkmark"></span>
              <span class="text">Activate login via SMS</span>
            </label>

            <label class="radios-input-group__input">
              <input type="radio" id="disable" name="setup" value="disable" />
              <span class="checkmark"></span>
              <span class="text">Disable all</span>
            </label>
          </div>
        </div>

        <label class="tos">
          <input type="checkbox" />
          <span class="checkmark"></span>
          <span class="text">I HAVE READ AND ACCEPT THE GENERAL TERMS AND CONDITIONS</span>
        </label>
      </template>
      <button type="submit" class="submit-btn" @click.prevent="onClick">
        <i v-if="currentStep !== 1" class="white-icon"></i>
        {{ currentStep === 1 ? 'GO TO NEXT STEP' : 'REGISTER NOW' }}
      </button>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import InputElement from '@/components/InputElement.vue'

const currentStep = ref(2)
const onClick = (): void => {
  console.log('1')
}
</script>

<style scoped lang="scss">
.container {
  background-color: $blue-1;
  display: flex;
  align-items: center;
  height: 100%;
  justify-content: center;
  padding: 1rem;
}

.form {
  background-color: $gray-1;
  border-radius: 30px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  max-width: 1400px;
  padding: 1.6rem 1.6rem 3.35rem;
  width: 97.23%;

  .input-group-container {
    display: flex;
    gap: 16px;
    justify-content: space-between;
    flex-wrap: wrap;
  }

  .radios {
    border-radius: 30px;
    border: 1px solid $gray-2;

    h3.radios-title {
      background-color: #d7d7d7;
      border-radius: 30px 30px 0 0;
      color: $dark-1;
      font-size: 0.95rem;
      margin: 0;
      padding: 1.7rem 1.9rem;
      text-align: center;
    }

    .radios-input-group {
      align-items: center;
      display: flex;
      gap: 0.85rem;
      flex-direction: column;
      justify-content: space-between;
      padding: 2rem 8rem 2rem 2rem;

      @media screen and (min-width: $breakpoint-sm) {
        flex-direction: row;
      }

      .radios-input-group__input {
        align-items: center;
        display: flex;
        position: relative;
        cursor: pointer;
        font-size: 22px;
        user-select: none;
        width: 100%;

        @media screen and (min-width: $breakpoint-sm) {
          width: auto;

          &:not(:first-child)::before {
            background-color: $gray-2;
            content: '';
            display: block;
            width: 4px;
            left: -15px;
            position: absolute;
            height: 3.75rem;
          }
        }

        input {
          cursor: pointer;
          margin: 0;
          opacity: 0;
          position: absolute;

          &:checked ~ .checkmark::before {
            border-radius: 50%;
            content: '';
            display: block;
            background-color: $dark-1;
            height: 0.5rem;
            left: 50%;
            width: 0.5rem;
            position: absolute;
            top: 50%;
            transform: translate(-50%, -50%);
          }

          &:checked ~ .checkmark:after {
            display: block;
          }
        }

        .checkmark {
          border: 1px solid $dark-1;
          border-radius: 50%;
          position: absolute;
          top: 50%;
          transform: translateY(-50%);
          height: 1.25rem;
          left: 0;
          width: 1.2rem;

          &:after {
            content: '';
            position: absolute;
            display: none;
          }
        }

        .text {
          font-size: 1rem;
          padding-left: 1.75rem;
        }
      }
    }
  }

  .submit-btn {
    align-items: center;
    border: 0;
    background-color: $green-1;
    border-radius: 45px;
    cursor: pointer;
    color: $white-1;
    display: flex;
    font-size: 1rem;
    line-height: 1.05;
    padding: 1.75rem;
    justify-content: center;
    transition: opacity 0.5s ease-out;

    &:hover {
      opacity: 0.85;
    }

    .white-icon {
      display: block;
      background: url('@/assets/icons/generic_icon_white.svg') center/cover no-repeat;
      height: 1.5rem;
      margin-right: 0.9rem;
      width: 1.5rem;
    }
  }

  .tos {
    color: $blue-1;
    cursor: pointer;
    display: flex;
    font-size: 1rem;
    margin: 0 auto;
    position: relative;
    user-select: none;
  }

  /* Hide the browser's default checkbox */
  .tos input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }

  /* Create a custom checkbox */
  .tos .checkmark {
    position: absolute;
    top: 0;
    left: -30px;
    height: 1.25rem;
    width: 1.25rem;
    background: url('@/assets/icons/hexagon.svg');
  }

  /* Show the checkmark when checked */
  .tos input:checked ~ .checkmark {
    background: url('@/assets/icons/hexagon-check.svg');
  }
}
</style>
