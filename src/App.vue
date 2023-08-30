<script>
import axios from 'axios';
import Sidebar from "@/components/Sidebar.vue";
import sunIcon from '@/assets/sun.png';
import moonIcon from '@/assets/moon.png';

export default {
  components: {
    Sidebar
  },
  data() {
    return {
      isDarkTheme: false,
      isLoading: false,
      userQuestion: '',
      backendUrl: 'https://legal-aid-chatbot-2ba2b124b39a.herokuapp.com/ask',
      responseData: null,
      userQuestions: [
        // "How does this work?",
        // "Can you provide an example?",
        // "Who is a lawyer?",
        // "What is a court?",
        // "Explain term 'tribunal'."
      ]
    };
  },
  computed: {
    toggleIcon() {
      return this.isDarkTheme ? sunIcon : moonIcon;
    }
  },
  methods: {
    async sendQuestion() {
      try {
        this.isLoading = true;
        const response = await axios.post(this.backendUrl, { question: this.userQuestion });
        this.responseData = response.data.answer;
        this.isLoading = false;
        this.userQuestions.push(this.userQuestion);
      } catch (error) {
        console.error(error);
        this.isLoading = false;
      }
    },
    clearText() {
      this.userQuestion = '';
    },
    deleteUserQuestion(index) {
      this.userQuestions.splice(index, 1);
    },
    toggleTheme() {
      this.isDarkTheme = !this.isDarkTheme;
      document.body.setAttribute('data-bs-theme', this.isDarkTheme ? 'dark' : 'light');
    }
  },
};
</script>

<template>
  <body>
    <header></header>
    <main class="">
      <div class="d-flex">
        <Sidebar :userQuestions="userQuestions" @deleteQuestion="deleteUserQuestion" />
        <div class="d-flex flex-fill flex-column align-items-center justify-content-center" style="min-height: 100vh;">
          <div>
            <button class="btn position-fixed top-0 end-0 m-3" @click="toggleTheme">
              <img :src="toggleIcon" alt="Toggle Theme" />
            </button>
          </div>
          <div>
            <img src="./assets/balance.png" alt="No image found">
          </div>
          <div class="mb-3 mt-5">
            <h1 class="display-4">Hi, this is <span class="text-warning fw-bold">Legal Aid Chatbot</span></h1>
            <h3 class="lead text-center"><em>Ask me something</em></h3>
          </div>
          <div class="my-3">
            <textarea class="form-control-lg" rows="5" cols="50" v-model="userQuestion"
                      placeholder="Type your question"/>
          </div>
          <div class="my-3">
            <button class="btn btn-lg btn-danger mx-2" @click="clearText">Clear</button>
            <button :disabled="isLoading" class="btn btn-lg btn-success mx-2" @click="sendQuestion">Ask</button>
          </div>
          <div v-if="isLoading" class="my-3 spinner-border text-primary" role="status">
            <span class="visually-hidden">Loading...</span>
          </div>
          <div v-else class="mx-5 my-3 px-5 justified-text">
            <p class="lead">{{ responseData }}</p>
          </div>
        </div>
      </div>
    </main>
    <footer></footer>
  </body>
</template>

<style scoped>
.justified-text {
  text-align: justify;
  word-spacing: 3px;
}
</style>
