<script setup lang="ts">
/**
 *
 */
documentTitle('Proyecto')
definePageMeta({
  layout: 'admin-layout',
})

/**
 *
 */
const startTts = (text: string) => {
  const utterance = new SpeechSynthesisUtterance(text)
  const audioContext = new AudioContext()
  const dest = audioContext.createMediaStreamDestination()

  const synth = window.speechSynthesis
  const source = audioContext.createMediaStreamSource(dest.stream)

  utterance.onstart = function () {
    const mediaRecorder = new MediaRecorder(dest.stream)
    let chunks: Blob[] = []

    mediaRecorder.ondataavailable = function (event) {
      if (event.data.size > 0) {
        chunks.push(event.data)
      }
    }

    mediaRecorder.onstop = function () {
      const blob = new Blob(chunks, { type: 'audio/mp3' })
      const audioURL = window.URL.createObjectURL(blob)
      console.log(audioURL)
    }

    mediaRecorder.start()
    utterance.onend = function () {
      mediaRecorder.stop()
    }
  }
  utterance.volume = 0.2
  synth.speak(utterance)
}
</script>

<template>
  <div class="w-full container-sm ml-auto mr-auto space-y-4">
    <div>
      <VueBreadCrumb text="NoName / " />
      <VueBreadCrumb text="Detalle Proyecto " />
      <h1 class="text-2xl font-bold">TTS</h1>
      <p class="text-muted-foreground">Detalle de tu Proyecto</p>
    </div>

    <Card>
      <h1>TTS</h1>
      <Button @click.prevent="startTts('hello world')">Test</Button>
    </Card>
  </div>
</template>
