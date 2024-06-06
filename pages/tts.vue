<script setup lang="ts">
/**
 *
 */
documentTitle("Proyecto");
definePageMeta({
  layout: "admin-layout",
});
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

      <Button>Test</Button>
    </Card>
  </div>
</template>
