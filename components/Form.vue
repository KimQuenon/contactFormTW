<script setup>
    import { ref } from 'vue'
    // import VueHcaptcha from '@hcaptcha/vue3-hcaptcha';

    //init successMessage
    const successMessage = ref('')

    //init array of fields
    const fields = ref({
      name: { value: '', error: false, errorMessage: '' },
      surname: { value: '', error: false, errorMessage: '' },
      email: { value: '', error: false, errorMessage: '' },
      phone: { value: '', error: false, errorMessage: '' },
      message: { value: '', error: false, errorMessage: '' },
    })

    //fields validation + init errorMessags
    const validateField = (fieldName, errorMessage, validator) => {
      if (!validator(fields.value[fieldName].value)) {
        fields.value[fieldName].error = true
        fields.value[fieldName].errorMessage = errorMessage
      } else {
        fields.value[fieldName].error = false
        fields.value[fieldName].errorMessage = ''
      }
    }

    //error messages - inputs
    const handleInputName = () => {
      validateField('name', 'Le champs doit comporter plus de 2 caractères', (value) => value.length >= 3)
    }
  
    const handleInputSurname = () => {
      validateField('surname', 'Le champs doit comporter plus de 1 caractère', (value) => value.length >= 2)
    }

    const handleInputEmail = () => {
      validateField('email', 'L\'adresse email est invalide', (value) => validateEmail(value))
    }

    const handleInputPhone = () => {
      validateField('phone', 'Le numéro de téléphone est invalide', (value) => validatePhone(value))
    }

    const handleInputMessage = () => {
      validateField('message', 'Votre message doit comporter plus de 50 caractères', (value) => value.length >= 51)
    }

    //regex mail & phone
    const validateEmail = (email) => {
      const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
      return re.test(String(email).toLowerCase())
    }

    const validatePhone = (phone) => {
      const re = /^(\+\d{1,3}\s?)?((\(\d{1,3}\))|\d{1,3})[-.\s]?\d{2,4}[-.\s]?\d{2,4}[-.\s]?\d{2,4}$/;
      return re.test(String(phone)) && phone.length <= 15;
    }
  
    //submit form
    const handleSubmit = () => {
      handleInputName()
      handleInputSurname()
      handleInputEmail()
      handleInputPhone()
      handleInputMessage()

      // console.log('Données du formulaire:', {
      //   name: fields.value.name.value,
      //   surname: fields.value.surname.value,
      //   email: fields.value.email.value,
      //   phone: fields.value.phone.value,
      //   message: fields.value.message.value
      // })

      //0 error => form submitted + clear fields
      if (Object.values(fields.value).every(field => !field.error)) {

          successMessage.value = 'Votre message a bien été envoyé.'

          Object.keys(fields.value).forEach(field => {
            fields.value[field].value = ''
          })
      }

    }


</script>

<template>
    <div class="container mx-auto">
        <form @submit.prevent="handleSubmit" class="max-w-lg mx-auto mt-4">
            <div v-if="successMessage" class="bg-green-200 text-green-800 border border-green-600 rounded-md px-4 py-2 mb-4">
                {{ successMessage }}
            </div>
            <div class="mb-4">
                <label class="block mb-1">Nom<span class="text-red-600">*</span></label>  
                <input
                type="text"
                v-model="fields.name.value"
                @input="handleInputName"
                class="form-input block w-full border border-gray-300 rounded px-3 py-2"
                placeholder="Votre nom..."
                required="required"
                :class="{ 'border-red-600': fields.name.error }"
                >
                <div v-if="fields.name.error" class="text-red-600 text-sm mt-1">{{ fields.name.errorMessage }}</div>
            </div>
            <div class="mb-4">
                <label class="block mb-1">Prénom<span class="text-red-600">*</span></label>  
                <input
                    type="text"
                    v-model="fields.surname.value"
                    @input="handleInputSurname"
                    class="form-input block w-full border border-gray-300 rounded px-3 py-2"
                    placeholder="Votre prénom..."
                    required="required"
                    :class="{ 'border-red-600': fields.surname.error }"
                >
                <div v-if="fields.surname.error" class="text-red-600 text-sm mt-1">{{ fields.surname.errorMessage }}</div>
            </div>
            <div class="mb-4">
                <label class="block mb-1">Email<span class="text-red-600">*</span></label>
                <input
                    type="email"  
                    v-model="fields.email.value"
                    @input="handleInputEmail"
                    class="form-input block w-full border border-gray-300 rounded px-3 py-2"
                    placeholder="Votre adresse mail..."
                    required="required"
                    :class="{ 'border-red-600': fields.email.error }"
                >
                <div v-if="fields.email.error" class="text-red-600 text-sm mt-1">{{ fields.email.errorMessage }}</div>
            </div>
            <div class="mb-4">
                <label class="block mb-1">Téléphone<span class="text-red-600">*</span></label>
                <input
                    type="tel"  
                    v-model="fields.phone.value"
                    @input="handleInputPhone"
                    class="form-input block w-full border border-gray-300 rounded px-3 py-2"
                    placeholder="Votre numéro de téléphone..."
                    required="required"
                    :class="{ 'border-red-600': fields.phone.error }"
                >
                <div v-if="fields.phone.error" class="text-red-600 text-sm mt-1">{{ fields.phone.errorMessage }}</div>
            </div>
            <div class="mb-4">
                <label class="block mb-1">Message<span class="text-red-600">*</span></label>
                <textarea
                    type="text"
                    v-model="fields.message.value"
                    @input="handleInputMessage"
                    class="form-textarea block w-full border border-gray-300 rounded px-3 py-2"
                    placeholder="Tapez votre message..."
                    required="required"
                    :class="{ 'border-red-600': fields.message.error }"
                ></textarea>
                <div v-if="fields.message.error" class="text-red-600 text-sm mt-1">{{ fields.message.errorMessage }}</div>
            </div>
            <div>
                <p class="text-sm text-gray-600">(<span class="text-red-600">*</span>) Ce champ est obligatoire.</p>
            </div>
            <div class="mt-4">
                <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600 focus:outline-none focus:bg-blue-600">Envoyer</button>
            </div>
        </form>
    </div>
  </template>