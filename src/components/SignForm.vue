<template>
  <form
    @submit.prevent="onSubmit()"
    class="w-full h-auto max-w-xs p-4 bg-white rounded-md text-darkBlue"
  >
    <label v-if="type === 'signup'" for="username" class="flex flex-col mb-6"
      >Username
      <input
        type="text"
        name="username"
        placeholder="Enter your username..."
        class="py-2 pl-3 mt-1 border border-gray-200 rounded-lg"
        v-model="username"
      />
    </label>

    <label for="email" class="flex flex-col mb-6"
      >Email
      <input
        type="text"
        name="email"
        placeholder="Enter your email..."
        class="py-2 pl-3 mt-1 border border-gray-200 rounded-lg"
        v-model="email"
      />
    </label>

    <label for="password" class="flex flex-col"
      >Password
      <input
        type="password"
        name="password"
        placeholder="Enter your password..."
        class="py-2 pl-3 mt-1 border border-gray-200 rounded-lg"
        v-model="password"
      />
    </label>
    <p v-if="error">Please fill all fields</p>
    <p v-else-if="errorMsg" class="mt-6">{{ errorMsg }}</p>
    <button
      type="submit"
      class="w-full py-1 mt-8 text-lg font-semibold text-white uppercase rounded-md bg-pink hover:opacity-90"
    >
      {{ btnText }}
    </button>
  </form>
</template>

<script>
import { ref, computed } from "vue";

export default {
  name: "SignForm",
  props: {
    type: String,
    errorMsg: String,
  },
  setup(props, { emit }) {
    const username = ref("");
    const password = ref("");
    const email = ref("");
    const error = ref(false);

    const onSubmit = () => {
      let userData = {};

      switch (props.type) {
        case "signup":
          error.value = !(username.value && password.value && email.value);
          userData = {
            username: username.value,
            password: password.value,
            email: email.value,
          };
          break;
        case "signin":
          error.value = !(email.value && password.value);
          userData = {
            email: email.value,
            password: password.value,
          };
          break;
        default:
          throw new Error("Type form unknown");
      }

      !error.value && emit("onSubmit", userData);
    };

    return {
      error,
      username,
      password,
      email,
      onSubmit,
      btnText: computed(() => (props.type == "signup" ? "sign up" : "sign in")),
    };
  },
};
</script>
