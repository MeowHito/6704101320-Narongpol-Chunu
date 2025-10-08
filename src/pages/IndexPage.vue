<template>
  <q-page class="row justify-center q-pa-lg">
    <div class="col-12 col-md-6 col-lg-4">
      <q-card>
        <q-card-section class="text-h5 text-weight-medium">Basic Information</q-card-section>

        <q-separator inset />

        <q-card-section>
          <q-form ref="formRef" class="q-gutter-md" @submit.prevent="onSubmit" @reset.prevent="onReset">
            <q-input
              v-model="form.name"
              label="Your name"
              filled
              hint="Name and surname"
              lazy-rules
              :rules="nameRules"
              clearable
            />

            <q-input
              v-model.number="form.age"
              label="Your age"
              inputmode="numeric"
              filled
              hint="Age in years"
              lazy-rules
              :rules="ageRules"
              clearable
            />

            <q-toggle
              v-model="form.accepted"
              label="I accept the terms and conditions"
              color="primary"
              :rules="[(val) => val || 'You must accept the terms']"
            />

            <div class="row q-col-gutter-sm">
              <div class="col">
                <q-btn type="submit" label="Submit" color="primary" class="full-width" />
              </div>
              <div class="col">
                <q-btn type="reset" label="Reset" color="secondary" outline class="full-width" />
              </div>
            </div>
          </q-form>
        </q-card-section>
      </q-card>
    </div>
  </q-page>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { useQuasar } from 'quasar'

const $q = useQuasar()
const formRef = ref(null)

const initialState = () => ({
  name: '',
  age: '',
  accepted: false,
})

const form = reactive(initialState())

const nameRules = [
  (val) => (val && val.trim().length > 0) || 'Please type something',
]

const ageRules = [
  (val) => (val !== null && val !== undefined && `${val}`.trim().length > 0) || 'Please type your age',
  (val) => {
    const numeric = Number(val)
    return (!Number.isNaN(numeric) && numeric > 0 && numeric < 150) || 'Please type a valid age'
  },
]

const onSubmit = async () => {
  const valid = await formRef.value?.validate()
  if (!valid) return

  $q.notify({
    type: 'positive',
    message: `Thanks, ${form.name}! Your form has been submitted.`,
  })
}

const onReset = () => {
  Object.assign(form, initialState())
  formRef.value?.resetValidation()
}
</script>