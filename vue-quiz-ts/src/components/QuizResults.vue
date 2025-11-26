<template>
  <div class="results" v-if="show">
    <h2>Результат</h2>
    <p>
      Ви відповіли правильно на
      <strong>{{ correct }}</strong> із
      <strong>{{ total }}</strong> запитань.
    </p>
    <p v-if="total > 0">
      Ваш відсоток: {{ percentage.toFixed(0) }}%.
    </p>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue'

export default defineComponent({
  name: 'QuizResults',
  props: {
    correct: {
      type: Number,
      required: true
    },
    total: {
      type: Number,
      required: true
    },
    show: {
      type: Boolean,
      default: false
    }
  },
  setup(props) {
    const percentage = computed(() => {
      if (!props.total) {
        return 0
      }
      return (props.correct / props.total) * 100
    })

    return {
      percentage
    }
  }
})
</script>

<style scoped>
.results {
  margin-top: 24px;
  padding: 16px;
  border-radius: 8px;
  background-color: #e8f5e9;
  border: 1px solid #c8e6c9;
}
</style>