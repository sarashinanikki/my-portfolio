<template>
  <v-app>
    <v-alert
      v-model="successAlert"
      dismissible
      type="success"
    >
      メッセージの送信が完了しました
    </v-alert>
    <v-alert
      v-model="errorAlert"
      dismissible
      type="error"
    >
      メッセージの送信に失敗しました
    </v-alert>
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
        color="warning"
        block
        :disabled="!valid || loading"
        :loading="loading"
        @click="send"
      >
        Send
      </v-btn>
    </v-form>
  </v-app>
</template>

<script>
  import axios from 'axios';
  const client = axios.create({
    baseURL: 'https://fb4wnrl507.execute-api.ap-northeast-1.amazonaws.com/',
    headers: {'Accept': '*/*', 'Content-Type': 'application/json'}
  });

  export default {
    data: () => ({
      valid: true,
      loading: false,
      successAlert: false,
      errorAlert: false,
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
      send () {
        this.loading = true;
        console.log(this.name);
        console.log(this.email);
        console.log(this.message);
        client.post('/api/line', {
            name: this.name,
            email: this.email,
            message: this.message
          })
          .then((res) => {
            console.log('ok');
            this.loading = false;
            this.successAlert = true;
          })
          .catch((err) => {
            console.error(err);
            this.loading = false;
            this.errorAlert = true;
          })
      }
    },
  }
</script>