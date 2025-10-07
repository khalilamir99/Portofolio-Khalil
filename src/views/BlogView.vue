<template>
  <div class="container mx-auto p-3 md:p-8 text-white">
    <div class="flex flex-col md:flex-row relative">
      <!-- Bagian kiri: Form kontak -->
      <div class="w-full md:w-2/3">
        <div class="flex flex-col gap-6 md:px-20 fade-zoom-up">
          <h1 class="text-2xl font-bold text-amber-300">Contact Me</h1>
          <p class="text-slate-300">
            Have a question, idea, or just want to say hello? Feel free to reach
            out using the form below.
          </p>

          <!-- Form Web3Forms -->
          <form
            @submit.prevent="sendMessage"
            class="flex flex-col gap-4 bg-[#1e1e1f] border border-[#383838] rounded-xl p-6"
          >
            <!-- Hidden Access Key -->
            <input type="hidden" name="access_key" :value="accessKey" />

            <div>
              <label class="block text-sm mb-1 text-slate-300">Name</label>
              <input
                v-model="form.name"
                name="name"
                type="text"
                class="w-full p-2 rounded-md bg-[#282828] border border-[#383838] focus:border-amber-200 outline-none"
                required
              />
            </div>

            <div>
              <label class="block text-sm mb-1 text-slate-300">Email</label>
              <input
                v-model="form.email"
                name="email"
                type="email"
                class="w-full p-2 rounded-md bg-[#282828] border border-[#383838] focus:border-amber-200 outline-none"
                required
              />
            </div>

            <div>
              <label class="block text-sm mb-1 text-slate-300">Message</label>
              <textarea
                v-model="form.message"
                name="message"
                rows="5"
                class="w-full p-2 rounded-md bg-[#282828] border border-[#383838] focus:border-amber-200 outline-none"
                required
              ></textarea>
            </div>

            <button
              type="submit"
              :disabled="isLoading"
              class="bg-amber-300 text-black font-semibold rounded-md py-2 px-4 hover:bg-amber-400 transition-all disabled:opacity-50 disabled:cursor-not-allowed"
            >
              {{ isLoading ? "Sending..." : "Send Message" }}
            </button>
          </form>

          <!-- Pesan sukses atau error -->
          <p v-if="successMessage" class="text-green-400 mt-3">
            {{ successMessage }}
          </p>
          <p v-if="errorMessage" class="text-red-400 mt-3">
            {{ errorMessage }}
          </p>
        </div>
      </div>

      <!-- Bagian kanan: Info -->
      <div class="w-full md:w-1/3 h-fit p-8 md:sticky md:top-24">
        <div class="flex flex-col text-left">
          <div
            class="bg-clip-text bg-gradient-to-r from-slate-100 to-amber-300 text-transparent"
          >
            Let's connect and collaborate!
          </div>
          <div class="h-[1px] mt-7 mb-7 w-20 bg-amber-200"></div>

          <div class="hidden md:block text-sm text-slate-300 space-y-2">
            <p><strong>Email:</strong> khalilamir230701@gmail.com</p>
            <p><strong>Location:</strong> Indonesia</p>
            <p>
              <strong>Available for:</strong> Work, Freelance & Collaboration
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      accessKey: "a986c565-e3cc-4ab0-9e4d-9debf997d1af", // ganti dengan Access Key kamu
      form: { name: "", email: "", message: "" },
      isLoading: false,
      successMessage: "",
      errorMessage: "",
    };
  },
  methods: {
    async sendMessage() {
      this.isLoading = true;
      this.successMessage = "";
      this.errorMessage = "";

      try {
        const res = await fetch("https://api.web3forms.com/submit", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            access_key: this.accessKey,
            name: this.form.name,
            email: this.form.email,
            message: this.form.message,
          }),
        });

        const data = await res.json();

        if (data.success) {
          this.successMessage =
            "✅ Thank you! Your message has been sent successfully.";
          this.form = { name: "", email: "", message: "" };
        } else {
          this.errorMessage = "❌ Failed to send message. Please try again.";
        }
      } catch (error) {
        this.errorMessage =
          "⚠️ An error occurred. Please check your connection.";
      } finally {
        this.isLoading = false;
        setTimeout(() => {
          this.successMessage = "";
          this.errorMessage = "";
        }, 4000);
      }
    },
  },
};
</script>

<style scoped>
@keyframes fadeZoomUp {
  0% {
    opacity: 0;
    transform: scale(0.5);
  }
  100% {
    opacity: 1;
    transform: scale(1);
  }
}
.fade-zoom-up {
  animation: fadeZoomUp 1s ease-in-out;
}
</style>
