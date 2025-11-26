<template>
  <main class="app">
    <h1>Тест: ділова комунікація</h1>

    <section>
      <QuestionItem
        v-for="(question, index) in questions"
        :key="question.id"
        :question="question"
        :index="index"
        v-model="selectedAnswers[index]"
      />
    </section>

    <button
      class="submit-btn"
      :disabled="!allAnswered"
      @click="submitQuiz"
    >
      Показати результат
    </button>

    <QuizResults
      :correct="correctCount"
      :total="questions.length"
      :show="isSubmitted"
    />
  </main>
</template>

<script lang="ts">
import { defineComponent, reactive, ref, computed } from 'vue'
import QuestionItem from './components/QuestionItem.vue'
import QuizResults from './components/QuizResults.vue'
import type { Question } from './types'

export default defineComponent({
  name: 'App',
  components: { QuestionItem, QuizResults },

  setup() {
    const questions: Question[] = reactive([
      { id: 1, text: 'Що таке ділова телефонна розмова?', options: [
        'Будь-який дзвінок друзям',
        'Спілкування з використанням телефону з діловою метою',
        'Розмова в чаті'
      ], correctIndex: 1 },

      { id: 2, text: 'Що з наведеного є прикладом ділового етикету?', options: [
        'Говорити нечітко і швидко',
        'Представитися на початку розмови',
        'Перебивати співрозмовника'
      ], correctIndex: 1 },

      { id: 3, text: 'Скільки приблизно триває стандартна ділова телефонна розмова?', options: [
        '1–2 хвилини', '3–5 хвилин', 'понад 15 хвилин'
      ], correctIndex: 1 },

      { id: 4, text: 'Хто за правилами етикету має першим завершувати розмову?', options: [
        'Той, хто телефонував',
        'Співрозмовник з вищим статусом',
        'Секретар'
      ], correctIndex: 1 },

      { id: 5, text: 'Що варто зробити, якщо співрозмовник не має часу говорити?', options: [
        'Наполягати на розмові',
        'Кинути слухавку',
        'Запропонувати передзвонити у зручний час'
      ], correctIndex: 2 }
    ])

    const selectedAnswers = reactive<(number | null)[]>(questions.map(() => null))

    // 🔥 ВСЕ У ref !!!
    const isSubmitted = ref(false)
    const correctCount = ref(0)

    const allAnswered = computed(() =>
      selectedAnswers.every((a) => a !== null)
    )

    const submitQuiz = () => {
      let count = 0
      questions.forEach((q, i) => {
        if (selectedAnswers[i] === q.correctIndex) count++
      })

      correctCount.value = count
      isSubmitted.value = true
    }

    return {
      questions,
      selectedAnswers,
      isSubmitted,
      correctCount,
      allAnswered,
      submitQuiz
    }
  }
})
</script>



<style scoped>
.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 24px;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
}

h1 {
  text-align: center;
  margin-bottom: 24px;
}

.submit-btn {
  margin-top: 16px;
  padding: 10px 18px;
  border-radius: 6px;
  border: none;
  cursor: pointer;
  font-size: 14px;
}

.submit-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}
</style>