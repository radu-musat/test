<template>
  <div class="container">
    <div class="form-wrapper">
      <form class="form">
        <template v-if="currentStep === 1">
          <!-- Step 1 Form Fields -->
          <div class="input-group-container">
            <input-element v-model="state.name" :errors="v$.name.$errors" />
            <input-element
              placeholder="Your Surname"
              v-model="state.surName"
              :errors="v$.surName.$errors"
            />
          </div>
          <div class="input-group-container">
            <input-element
              placeholder="Your Username"
              v-model="state.userName"
              :errors="v$.userName.$errors"
            />
            <input-element
              placeholder="E-mail"
              type="email"
              v-model="state.email"
              :errors="v$.email.$errors"
            />
          </div>
          <div class="input-group-container">
            <input-element
              placeholder="Your Password"
              type="password"
              @toggle-password-visibility="displayPassword = !displayPassword"
              :password-is-visible="displayPassword"
              v-model="state.password"
              :errors="v$.password.$errors"
            />
            <input-element
              placeholder="Reenter password"
              type="password"
              @toggle-password-visibility="displayPassword = !displayPassword"
              :password-is-visible="displayPassword"
              v-model="state.confirmPassword"
              :errors="v$.confirmPassword.$errors"
            />
          </div>
        </template>
        <template v-else>
          <!-- Step 2 Form Fields -->
          <div class="input-group-container">
            <input-element
              type="number"
              placeholder="Identification Number"
              v-model="state.identificationNumber"
              :errors="v$.identificationNumber.$errors"
            />
            <input-element
              type="tel"
              placeholder="Telephone"
              v-model="state.telephone"
              :errors="v$.telephone.$errors"
              :tel-prefix="'50'"
            />
          </div>
          <div class="input-group-container">
            <input-element
              placeholder="Security Question"
              v-model="state.securityQuestion"
              :errors="v$.securityQuestion.$errors"
            />
            <input-element
              placeholder="Your Answer"
              v-model="state.securityAnswer"
              :errors="v$.securityAnswer.$errors"
            />
          </div>
          <div class="radios">
            <h3 class="radios-title">LOGIN SETUP MENU</h3>
            <div class="radios-input-group">
              <label class="radios-input-group__input">
                <input
                  type="radio"
                  id="login"
                  checked
                  name="setup"
                  value="login"
                  v-model="state.setup"
                />
                <span class="checkmark"></span>
                <span class="text">Active login email</span>
              </label>
              <label class="radios-input-group__input">
                <input type="radio" id="sms" name="setup" value="sms" v-model="state.setup" />
                <span class="checkmark"></span>
                <span class="text">Activate login via SMS</span>
              </label>
              <label class="radios-input-group__input">
                <input
                  type="radio"
                  id="disable"
                  name="setup"
                  value="disable"
                  v-model="state.setup"
                />
                <span class="checkmark"></span>
                <span class="text">Disable all</span>
              </label>
            </div>
          </div>
          <label class="tos">
            <input type="checkbox" v-model="state.acceptTerms" />
            <span class="checkmark"></span>
            <span class="text">I HAVE READ AND ACCEPT THE GENERAL TERMS AND CONDITIONS</span>
          </label>
          <small class="error-msg" v-if="!state.acceptTerms && submitAttempt">
            Please accept our terms of service before continuing
          </small>
        </template>
        <button type="submit" class="submit-btn" @click.prevent="onClick">
          <i v-if="currentStep !== 1" class="white-icon"></i>
          {{ currentStep === 1 ? 'GO TO NEXT STEP' : 'REGISTER NOW' }}
        </button>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import useVuelidate from '@vuelidate/core'
import { required, minLength, email, sameAs } from '@vuelidate/validators'

import { ref, reactive, computed } from 'vue'
import InputElement from '@/components/InputElement.vue'

const displayPassword = ref(false)
const currentStep = ref(1)
const submitAttempt = ref(false)

const state = reactive({
  name: null,
  surName: '',
  userName: '',
  email: '',
  password: '',
  confirmPassword: '',
  identificationNumber: '',
  telephone: '',
  securityQuestion: '',
  securityAnswer: '',
  setup: 'login',
  acceptTerms: false
})

const rules = {
  name: { required, minLength: minLength(2) },
  surName: { required, minLength: minLength(2) },
  userName: { required, minLength: minLength(3) },
  email: { required, email },
  password: { required, minLength: minLength(6) },
  confirmPassword: {
    required,
    sameAsPassword: sameAs(computed(() => state.password)),
    minLength: minLength(6)
  },
  identificationNumber: { required, minLength: minLength(6) },
  telephone: { required },
  securityQuestion: { required },
  securityAnswer: { required }
}

const v$ = useVuelidate<typeof state>(rules, state)
const step1isValid = computed(() => {
  return (
    !v$.value.name.$errors.length &&
    !v$.value.surName.$errors.length &&
    !v$.value.userName.$errors.length &&
    !v$.value.email.$errors.length &&
    !v$.value.password.$errors.length &&
    !v$.value.confirmPassword.$errors.length
  )
})

const step2isValid = computed(() => {
  return (
    !v$.value.identificationNumber.$errors.length &&
    !v$.value.telephone.$errors.length &&
    !v$.value.securityQuestion.$errors.length &&
    !v$.value.securityAnswer.$errors.length &&
    state.acceptTerms
  )
})

const onClick = (): void => {
  if (currentStep.value === 2) {
    submitAttempt.value = true
  }

  v$.value.$validate()
  if (step1isValid.value && currentStep.value === 1) {
    alert(JSON.stringify(state, null, 2))
    currentStep.value += 1
    v$.value.$reset()
  }

  if (step1isValid.value && step2isValid.value) {
    submitAttempt.value = false
    state.telephone = '50' + state.telephone
    alert(JSON.stringify(state, null, 2))
    return
  }
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

.form-wrapper {
  overflow-y: auto; /* Enable vertical scrolling */
  max-height: 90vh; /* Limit the height to 90% of viewport height */
  width: 100%;
  display: flex;
  justify-content: center;
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
}

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
  max-width: 70%;
  position: relative;
  user-select: none;

  @media screen and (min-width: $breakpoint-sm) {
    max-width: none;
  }
}

.tos input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.tos .checkmark {
  position: absolute;
  top: 0;
  left: -30px;
  height: 1.25rem;
  width: 1.25rem;
  background: url('@/assets/icons/hexagon.svg');
}

.tos input:checked ~ .checkmark {
  background: url('@/assets/icons/hexagon-check.svg');
}

.error-msg {
  color: $red-1;
  display: block;
  font-size: 10px;
  padding-top: 5px;
  text-align: center;

  @media screen and (min-width: $breakpoint-md) {
    font-size: 14px;
  }
}
</style>
