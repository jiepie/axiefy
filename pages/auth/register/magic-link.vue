<template>
  <v-form ref="form" @submit.prevent="sendMagicLink">
    <v-card-text class="mt-16">
      <v-text-field
        label="Email"
        type="email"
        v-model="form.email"
        :rules="rules.email"
        outlined
        dense
      ></v-text-field>
      <div class="d-flex justify-end">
        <v-btn type="submit" :loading="loading$" color="primary">
          Send Magic Link
        </v-btn>
      </div>
    </v-card-text>
  </v-form>
</template>

<script>
export default {
  head: {
    title: "Magic Link"
  },

  layout: "auth",

  auth: "guest",

  data: () => ({
    loading$: false,
    authMethod: null,
    form: {
      name: "",
      email: ""
    }
  }),

  created() {},

  computed: {
    rules() {
      return {
        email: [v => !!v || "Email is required"]
      };
    }
  },

  methods: {
    async sendMagicLink() {
      this.loading$ = true;

      try {
        const { email } = this.form;
        const response = await this.$supabase.auth.signIn({
          email
        });
        this.$log.info(response)
      } catch (error) {
        this.$toast.showUnexpectedError();
        this.$log.error(error);
      } finally {
        this.loading$ = false;
      }
    }
  }
};
</script>
