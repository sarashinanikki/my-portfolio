<template>
  <v-app>
    <v-form
      ref="form"
      v-model="valid"
      lazy-validation
    >
      <v-text-field
        v-model="name"
        :counter="32"
        :rules="nameRules"
        label="Name"
        required
        outlined
      ></v-text-field>

      <v-text-field
        v-model="email"
        label="E-mail"
        :rules="emailRules"
        type="email"
        required
        outlined
      ></v-text-field>

      <v-textarea
        v-model="message"
        label="Message"
        :counter="1000"
        :rules="messageRules"
        full-width
        outlined
      ></v-textarea>

      <v-btn
        :disabled="!valid"
        light="true"
        color="warning"
        block
        @click="validate"
      >
        Send
      </v-btn>
    </v-form>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 32) || 'Name must be less than 32 characters',
      ],
      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
      message: '',
      messageRules: [
          v => !!v || 'Message is required',
          v => (v && v.length <= 1000) || 'Message must be less than 1000 characters'
      ]
    }),

    methods: {
      validate () {
        this.$refs.form.validate()
      }
    },
  }
</script>