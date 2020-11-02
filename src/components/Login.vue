<template>
  <div class="flex justify-center h-screen items-center">
    <form
      @submit="
        (e) => {
          e.preventDefault();
        }
      "
      class="w-full h-full relative lg:h-auto lg:w-1/2 max-w-lg bg-gray-100 rounded-lg p-10"
    >
    <lang-gear class="absolute custom-position"></lang-gear>
      <div class="flex flex-wrap -mx-3 mb-6">
        <div class="w-full flex items-start flex-col px-3 mb-6 md:mb-0">
          <label
            class="block uppercase tracking-wide text-gray-700 font-bold mb-2"
            for="grid-first-name"
          >
            {{ $t("email") }}
          </label>
          <input
            @change="
              (e) => {
                checkEmail = re.test(String(email).toLowerCase());
              }
            "
            class="appearance-none block w-full bg-gray-200 text-gray-700 border rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
            id="grid-first-name"
            type="text"
            v-model="email"
            :class="!checkEmail ? 'border-red-500' : ''"
            :placeholder="$t('email')"
          />
          <p
            class="text-red-500 text-xs italic"
            :class="checkEmail ? 'hidden' : 'block'"
          >
            {{ $t("Please enter a valid email") }}
          </p>
        </div>
        <div class="w-full flex items-start flex-col px-3 mb-6 md:mb-0">
          <label
            class="block uppercase tracking-wide text-gray-700 font-bold mb-2"
            for="grid-first-name"
          >
            {{ $t("password") }}
          </label>
          <input
            @change="
              (e) => {
                checkPassword = password && true;
              }
            "
            class="appearance-none block w-full bg-gray-200 text-gray-700 border rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
            id="grid-first-name"
            type="password"
            v-model="password"
            :class="!checkPassword ? 'border-red-500' : ''"
            :placeholder="$t('password')"
          />
          <p
            class="text-red-500 text-xs italic"
            :class="checkPassword ? 'hidden' : 'block'"
          >
            {{ $t("validatePassword") }}
          </p>
        </div>
        <div class="flex w-full justify-evenly">
          <button
            type="submit"
            @click="login"
            class="md:w-32 bg-indigo-600 hover:bg-blue-dark text-white font-bold py-3 px-6 rounded-lg mt-3 hover:bg-indigo-500 transition ease-in-out duration-300"
          >
            {{ $t("login") }}
          </button>
        </div>
      </div>
      <p class="text-red-500 text-xs italic">
        {{ error }}
      </p>
    </form>
  </div>
</template>

<script>
import { db } from "../firebaseDB";
import LangGear from "./LangGear"

export default {
  components: {
    LangGear
  },
  data() {
    return {
      checkEmail: true,
      checkPassword: true,
      checkName: true,
      email: "",
      password: "",
      error: "",
      name: "",
      signup: false,
      // eslint-disable-next-line no-useless-escape
      re: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/,
    };
  },
  methods: {
    check() {
      console.log();
    },
    login() {
      if (!this.checkEmail && !this.checkPassword) return;
      if(this.email != "hamza@lovester.com") return;
      db.app
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then(() => this.$router.push({ name: "home" }))
        .catch(() => (this.error = this.$t("loginError")));
    },
  },
};
</script>

<style>
.custom-position{
  top: 5px;
  right: 5px;
}
</style>
