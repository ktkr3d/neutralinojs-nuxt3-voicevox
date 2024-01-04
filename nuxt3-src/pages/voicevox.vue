
<template>
  <div>
    <form @submit.prevent="submitForm">
      <v-table>
        <thead>
          <tr>
            <td>
              <v-container fluid>
                <v-textarea  id="text" v-model="formData.text" counter prepend-inner-icon="mdi-comment" class="mx-2" label="メッセージを入力..." rows="3"></v-textarea>
              </v-container>
            </td>
            <td><v-btn type="submit">音声合成</v-btn></td>
            <td></td>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(message, index) in messages" :key="index">
            <td>{{ message.text }}</td>
            <td><audio :src="message.url" controls/></td>
            <td><a :href="message.url" download title="ダウンロード"><v-btn elevation="8" fab icon="mdi-file-download"></v-btn></a></td>
          </tr>
        </tbody>
      </v-table>
    </form>
  </div>
</template>

<script setup>
import { ref } from "vue";

const formData = ref({
  text: "",
  url: "",
});

const messagesData = [];
const messages = ref(messagesData);

function addMessage() {
  if (formData.value.text) {
    messages.value.push({
      text: formData.value.text,
      url: formData.value.url,
    });
    formData.value.text = "";
  }
}

const submitForm = async () => {
  const { data: queryJson } = await useFetch("http://localhost:50021/audio_query", {
    method: "POST",
    query: { style_id: "1", text: formData.value.text },
  });
  const { data: audioData } = await useFetch("http://localhost:50021/synthesis", {
    method: "POST",
    query: { style_id: "1" },
    body: queryJson,
    responseType: "blob",
    onResponse({ request, response, options }) {
      formData.value.url = window.URL.createObjectURL(response._data);
    },
  });
  addMessage();
};
</script>

<style lang="css">
  td {
    padding: 16px !important;
  }
</style>
