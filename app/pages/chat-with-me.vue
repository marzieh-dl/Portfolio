<template>
  <UContainer class="py-20 flex flex-col items-center gap-10">
    <section
      class="flex h-full min-h-0 w-full flex-col gap-4 overflow-hidden border-y border-slate-200/70 bg-[linear-gradient(180deg,_rgba(255,255,255,0.98)_0%,_rgba(241,245,249,0.98)_100%)] px-5 py-4 shadow-[0_30px_100px_rgba(15,23,42,0.12)] backdrop-blur transition-colors dark:border-slate-800 dark:bg-[linear-gradient(180deg,_rgba(2,6,23,0.98)_0%,_rgba(15,23,42,0.98)_100%)] dark:shadow-[0_30px_100px_rgba(0,0,0,0.45)] sm:px-7 sm:py-4 lg:px-10 lg:py-5"
    >
      <div class="shrink-0 space-y-3 text-center">
        <div class="flex h-16 items-center justify-center overflow-hidden">
          <h1
            class="max-w-4xl text-2xl font-semibold leading-tight tracking-tight text-slate-950 dark:text-slate-50 sm:text-3xl"
          >
            {{ t('chat_with_me.title') }}
            <span
              class="text-base font-medium text-slate-500 dark:text-slate-400 sm:text-lg"
            >
              ({{ t('chat_with_me.eyebrow') }})
            </span>
          </h1>
        </div>

        <div class="grid grid-cols-1 gap-2.5 sm:grid-cols-2 xl:grid-cols-4">
          <UButton
            v-for="question in suggestedQuestions"
            :key="question.key"
            color="neutral"
            variant="soft"
            class="h-12 w-full rounded-full border border-slate-200 bg-white px-4 py-2 text-center text-xs leading-tight text-slate-700 shadow-sm transition hover:border-sky-200 hover:bg-sky-50 hover:text-sky-900 dark:border-slate-700 dark:bg-slate-900 dark:text-slate-200 dark:hover:border-sky-700 dark:hover:bg-slate-800 dark:hover:text-sky-200 sm:text-sm"
            @click="askPresetQuestion(question.key)"
          >
            {{ question.label }}
          </UButton>
        </div>
      </div>

      <div
        class="flex h-[30vw] overflow-y-auto flex-col rounded-[2.25rem] border border-slate-200 bg-white shadow-[0_18px_60px_rgba(15,23,42,0.08)] transition-colors dark:border-slate-800 dark:bg-slate-950 dark:shadow-[0_18px_60px_rgba(0,0,0,0.45)]"
      >
        <div
          class="border-b border-slate-200 bg-slate-50/80 px-5 py-4 dark:border-slate-800 dark:bg-slate-900/80 sm:px-7"
        >
          <p class="text-base text-slate-500 dark:text-slate-400">
            {{ t('chat_with_me.helper') }}
          </p>
        </div>

        <div class="flex min-h-0 flex-1 flex-col px-4 py-4 sm:px-6 sm:py-5">
          <div class="min-h-0 flex-1 space-y-5 overflow-y-auto pr-1">
            <template v-if="messages.length">
              <div
                v-for="message in messages"
                :key="message.id"
                class="flex"
                :class="
                  message.role === 'user' ? 'justify-end' : 'justify-start'
                "
              >
                <div
                  class="max-w-[90%] rounded-[1.75rem] px-5 py-4 shadow-sm sm:max-w-[75%]"
                  :class="
                    message.role === 'user'
                      ? 'bg-slate-950 text-white dark:bg-sky-400 dark:text-slate-950'
                      : 'border border-slate-200 bg-slate-50 text-slate-900 dark:border-slate-700 dark:bg-slate-900 dark:text-slate-100'
                  "
                >
                  <p
                    class="mb-2 text-xs font-semibold uppercase tracking-[0.2em]"
                    :class="
                      message.role === 'user'
                        ? 'text-slate-300 dark:text-slate-700'
                        : 'text-slate-500 dark:text-slate-400'
                    "
                  >
                    {{
                      message.role === 'user'
                        ? t('chat_with_me.you')
                        : t('chat_with_me.assistant')
                    }}
                  </p>
                  <p class="text-base leading-7">
                    {{ message.text }}
                  </p>
                </div>
              </div>
            </template>

            <div
              v-else
              class="flex h-full min-h-0 flex-col items-center justify-center rounded-[1.75rem] border border-dashed border-slate-300 bg-slate-50 px-8 text-center dark:border-slate-700 dark:bg-slate-900"
            >
              <p
                class="text-2xl font-semibold text-slate-900 dark:text-slate-100"
              >
                {{ t('chat_with_me.empty_title') }}
              </p>
              <p
                class="mt-3 max-w-lg text-base leading-7 text-slate-500 dark:text-slate-400"
              >
                {{ t('chat_with_me.empty_body') }}
              </p>
            </div>
          </div>

          <form
            class="mt-6 shrink-0 border-t border-slate-200 pt-5 dark:border-slate-800"
            @submit.prevent="submitQuestion"
          >
            <label
              class="mb-3 block text-base font-medium text-slate-700 dark:text-slate-200"
              for="chat-question"
            >
              {{ t('chat_with_me.input_label') }}
            </label>
            <div class="flex flex-col gap-4 sm:flex-row sm:items-stretch">
              <UInput
                id="chat-question"
                v-model="draftQuestion"
                :placeholder="t('chat_with_me.input_placeholder')"
                size="xl"
                class="flex-1"
                :ui="{
                  base: 'min-h-16 rounded-[1.5rem] border border-slate-200 bg-white px-5 text-base text-slate-900 shadow-sm dark:border-slate-700 dark:bg-slate-900 dark:text-slate-100',
                }"
              />
              <UButton
                type="submit"
                color="neutral"
                size="xl"
                class="min-h-16 rounded-[1.5rem] bg-slate-950 px-8 text-base justify-center text-white hover:bg-slate-800 dark:bg-sky-400 dark:text-slate-950 dark:hover:bg-sky-300"
              >
                {{ t('chat_with_me.send') }}
              </UButton>
            </div>
          </form>
        </div>
      </div>
    </section>
  </UContainer>
</template>

<script setup lang="ts">
type QuestionKey = 'experience' | 'technologies' | 'languages' | 'location'

type ChatMessage = {
  id: number
  role: 'user' | 'assistant'
  text: string
}

const { t } = useI18n()

const draftQuestion = ref('')
const messages = ref<ChatMessage[]>([])
const nextMessageId = ref(1)

const suggestedQuestions = computed(() => [
  {
    key: 'experience' as QuestionKey,
    label: t('chat_with_me.questions.experience'),
  },
  {
    key: 'technologies' as QuestionKey,
    label: t('chat_with_me.questions.technologies'),
  },
  {
    key: 'languages' as QuestionKey,
    label: t('chat_with_me.questions.languages'),
  },
  {
    key: 'location' as QuestionKey,
    label: t('chat_with_me.questions.location'),
  },
])

const defaultAnswers = computed<Record<QuestionKey, string>>(() => ({
  experience: t('chat_with_me.answers.experience'),
  technologies: t('chat_with_me.answers.technologies'),
  languages: t('chat_with_me.answers.languages'),
  location: t('chat_with_me.answers.location'),
}))

const technologyAnswers = computed<Record<string, string>>(() => ({
  react: t('chat_with_me.answers.react'),
  vue: t('chat_with_me.answers.vue'),
  angular: t('chat_with_me.answers.angular'),
  next: t('chat_with_me.answers.next'),
  typescript: t('chat_with_me.answers.typescript'),
  ngrx: t('chat_with_me.answers.ngrx'),
  redux: t('chat_with_me.answers.redux'),
  tanstack: t('chat_with_me.answers.tanstack'),
  vite: t('chat_with_me.answers.vite'),
}))

function addMessage(role: ChatMessage['role'], text: string) {
  messages.value.push({
    id: nextMessageId.value++,
    role,
    text,
  })
}

function askPresetQuestion(key: QuestionKey) {
  addMessage('user', t(`chat_with_me.questions.${key}`))
  addMessage('assistant', defaultAnswers.value[key])
}

function normalizeQuestion(value: string) {
  return value.trim().toLowerCase()
}

function getAnswerForQuestion(question: string) {
  const normalizedQuestion = normalizeQuestion(question)

  const technologyKeywords: Record<string, string[]> = {
    react: ['react'],
    vue: ['vue', 'vue.js', 'vuejs'],
    angular: ['angular'],
    next: ['next', 'next.js', 'nextjs'],
    typescript: ['typescript', 'type script'],
    ngrx: ['ngrx'],
    redux: ['redux'],
    tanstack: ['tanstack', 'react query', 'tanstack query'],
    vite: ['vite'],
  }

  for (const [key, keywords] of Object.entries(technologyKeywords)) {
    if (keywords.some((keyword) => normalizedQuestion.includes(keyword))) {
      return technologyAnswers.value[key]
    }
  }

  if (
    normalizedQuestion.includes('year') ||
    normalizedQuestion.includes('experience') ||
    normalizedQuestion.includes('jahr')
  ) {
    return defaultAnswers.value.experience
  }

  if (
    normalizedQuestion.includes('technolog') ||
    normalizedQuestion.includes('stack') ||
    normalizedQuestion.includes('framework') ||
    normalizedQuestion.includes('tool')
  ) {
    return defaultAnswers.value.technologies
  }

  if (
    normalizedQuestion.includes('language') ||
    normalizedQuestion.includes('speak') ||
    normalizedQuestion.includes('sprache') ||
    normalizedQuestion.includes('speak')
  ) {
    return defaultAnswers.value.languages
  }

  if (
    normalizedQuestion.includes('where') ||
    normalizedQuestion.includes('based') ||
    normalizedQuestion.includes('location') ||
    normalizedQuestion.includes('live') ||
    normalizedQuestion.includes('wo')
  ) {
    return defaultAnswers.value.location
  }

  return t('chat_with_me.fallback')
}

function submitQuestion() {
  const question = draftQuestion.value.trim()

  if (!question) {
    return
  }

  addMessage('user', question)
  addMessage('assistant', getAnswerForQuestion(question))
  draftQuestion.value = ''
}
</script>
