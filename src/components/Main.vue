<template>
  <div
    class="flex flex-col items-center h-full w-full overflow-auto justify-center"
  >
    <h1 class="text-3xl">Account generation</h1>
    <form
      @submit="
        (e) => {
          e.preventDefault();
        }
      "
      v-if="!submitted"
      class="w-full lg:w-1/2 max-w-lg bg-gray-100 rounded-lg p-10"
    >
      <div class="flex flex-wrap justify-center -mx-3 mb-6">
        <div class="w-full px-3 mb-6 md:mb-0">
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            for="grid-first-name"
          >
            {{ $t("number") }}
          </label>
          <input
            @change="
              (e) => {
                checkNumber = number > 0;
              }
            "
            class="appearance-none block w-full bg-gray-200 text-gray-700 border rounded py-3 px-4 mb-3 leading-tight focus:outline-none focus:bg-white"
            id="grid-first-name"
            type="number"
            v-model="number"
            :class="!checkNumber ? 'border-red-500' : ''"
            :placeholder="$t('number')"
          />
          <p
            class="text-red-500 text-xs italic"
            :class="checkName ? 'hidden' : 'block'"
          >
            {{ $t("incomplete") }}
          </p>
        </div>

        <button
          type="submit"
          @click="check"
          class="md:w-32 bg-indigo-600 hover:bg-blue-dark text-white font-bold py-3 px-6 rounded-lg mt-3 hover:bg-indigo-500 transition ease-in-out duration-300"
        >
          Generate accounts
        </button>
      </div>
      <p class="text-red-500 text-xs italic">
        {{ error }}
      </p>
    </form>
  </div>
</template>

<script>
import "@/assets/tailwind.css";
import { db } from "../firebaseDB";

export default {
  name: "HelloWorld",
  data() {
    return {
      checkNumber: true,
      number: 0,
    };
  },
  methods: {
    async check() {
      this.checkNumber = this.number > 0;
      const filename = "result.csv";
      let content = "";

      for (let i = 0; i < this.number; i++) {
        let date = "" + Date.now();
        date = date.substring(2) + i;
        let result = "";
        let characters =
          "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
        let charactersLength = characters.length;
        for (let j = 0; j < 8; j++) {
          result += characters.charAt(
            Math.floor(Math.random() * charactersLength)
          );
        }
        await db.app
          .auth()
          .createUserWithEmailAndPassword(
            "user" + date + "@lovester.com",
            result
          );
        content += "user" + date + "@lovester.com," + result + "\n";
      }

      const blob = new Blob([content], {
        type: "text/plain;charset=utf-8",
      });
      let link = document.createElement("a");
      link.href = window.URL.createObjectURL(blob);
      link.download = filename;

      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    },
  },
  async mounted() {
    if (!db.app.auth().currentUser) this.$router.push({ name: "login" });
  },
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.blur {
  filter: blur(3px);
  transition: 0.5s;
}
.position-custom {
  top: 2rem;
}
</style>
