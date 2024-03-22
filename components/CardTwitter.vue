<script setup lang="ts">
// import { AsyncState } from "~~/types";
const props = defineProps<{
    url: string;
    temperature: number
}>();
const { chat, state, firstMessage } = useChatAi({ agent: "twitter" })
const announcement = computed(() => firstMessage.value?.content);
// const state = ref<AsyncState>("complete");
const generate = nextTick(() => chat(props));
defineExpose({
    generate,
});
const postURL = computed(
    () =>
        `https://twitter.com/intent/tweet?text=${encodedURIComponent(
            announcement.value || ""
        )}`
);
</script>
<template>
    <CardGeneric title="Twitter" :state="state" :body="announcement" class="mb-10">
        <div class="flex w-full justify-between items-center">
            <div class="text-xs">
                Character Count:
                <strong>{{ announcement.length }}</strong>
            </div>
            <div>
                <button class="btn btn-neutral" @click="generate()">Regenerate</button>
                <a :href="postURL" class="btn btn-primary" target="_blank">Post</a>
            </div>
        </div>
    </CardGeneric>
</template>
