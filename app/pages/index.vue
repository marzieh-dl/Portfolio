<template>
  <UContainer class="py-20 flex flex-col items-center gap-10">
    <HeroTitle :my-name="aboutMe.name" :text="aboutMe.text" />

    <div class="flex flex-col items-center gap-4">
      <HeroResumeDownload />

      <UButton color="neutral" variant="soft" :to="localePath('/chat-with-me')">
        {{ $t('chat_with_me.nav') }}
      </UButton>
    </div>
  </UContainer>
</template>

<script setup lang="ts">
const localePath = useLocalePath()

const aboutMe = {
  name: 'Marzieh',
  text: $t('aboutme.text'),
}
</script>
