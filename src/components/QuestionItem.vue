<template>
  <div class="question-block">
    <h3 class="question-title">
      {{ index + 1 }}. {{ question.text }}
    </h3>

    <ul class="options-list">
      <li
        v-for="(option, optionIndex) in question.options"
        :key="optionIndex"
        class="option-item"
      >
        <label>
          <input
  type="radio"
  :name="'q-' + question.id"
  :value="optionIndex"
  @change="onChange(optionIndex)"
  :checked="modelValue === optionIndex"
/>


          <span>{{ option }}</span>
        </label>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'
import type { Question } from '../types'

export default defineComponent({
  name: 'QuestionItem',
  props: {
    question: {
      type: Object as PropType<Question>,
      required: true
    },
    index: {
      type: Number,
      required: true
    },
    modelValue: {
      type: Number as PropType<number | null>,
      default: null
    }
  },
  emits: ['update:modelValue'],
  setup(props, { emit }) {
    const onChange = (optionIndex: number) => {
      emit('update:modelValue', optionIndex)
    }

    return {
      onChange
    }
  }
})
</script>

<style scoped>
.question-block {
  margin-bottom: 20px;
  padding: 16px;
  border-radius: 8px;
  border: 1px solid #ddd;
  background-color: #fafafa;
}

.question-title {
  margin: 0 0 8px;
  font-size: 16px;
}

.options-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.option-item + .option-item {
  margin-top: 4px;
}

label {
  cursor: pointer;
}
</style>