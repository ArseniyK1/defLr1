<template>
  <q-page class="flex flex-center bg-blue-1">
    <q-form
      style="border: 1px solid black; border-radius: 15px"
      class="q-pa-lg bg-brown-1"
    >
      <div class="row" style="width: 50vw; height: 70vh">
        <div class="col-6 q-pr-sm" style="border-right: 1px solid black">
          <div class="text-h4 text-bold">Гаммирование</div>
          <q-separator color="black" w />
          <div class="text-h5">Шифрование</div>
          <div class="shifr">
            <q-input
              @update:model-value="encryption_gamma"
              v-model="start_text_gamma"
              label="Исходный текст"
              outlined
            ></q-input>
            <q-input
              v-model="key_gamma"
              label="Значение Гаммы"
              readonly
              style="margin: 10px 0"
              outlined
            ></q-input>
            <q-input
              v-model="result_gamma"
              readonly
              label="Результат"
              outlined
            ></q-input>
          </div>
          <div class="deshifr">
            <p class="text-h5">Дешифрование</p>
            <q-input
              v-model="finish_text_gamma"
              @update:model-value="decryption_gamma"
              label="Введите кодированный текст"
              outlined
            ></q-input>
            <q-input
              v-model="finish_key_gamma"
              label="Значение Гаммы"
              @update:model-value="decryption_gamma"
              style="margin: 10px 0"
              outlined
            ></q-input>
            <q-input
              v-model="finish_result_gamma"
              label="Результат"
              outlined
              readonly
            ></q-input>
          </div>
        </div>
        <div class="q-pl-sm col-6">
          <div class="text-h4 text-bold">Метод Виженера</div>
          <q-separator color="black" w />
          <div class="text-h5">Шифрование</div>

          <div class="shifr">
            <q-input
              @update:model-value="encryption"
              v-model="start_text"
              label="Введите исходный текст"
              outlined
              style="width: 100%"
            ></q-input>
            <q-input
              @update:model-value="encryption"
              v-model="key"
              label="Введите ключ"
              outlined
              style="margin: 10px 0"
            ></q-input>
            <q-input
              v-model="result"
              label="Результат"
              outlined
              readonly
            ></q-input>
          </div>

          <div class="deshifr">
            <p class="text-h5">Дешифрование</p>
            <q-input
              @update:model-value="decryption"
              v-model="finish_text"
              label="Введите кодированный текст"
              outlined
              style="width: 100%"
            ></q-input>
            <q-input
              @update:model-value="decryption"
              v-model="finish_key"
              label="Введите ключ"
              outlined
              style="margin: 10px 0"
            ></q-input>
            <q-input
              v-model="finish_result"
              label="Результат"
              outlined
              readonly
            ></q-input>
          </div>
        </div>
      </div>
    </q-form>
  </q-page>
</template>

<script setup>
import { ref } from "vue";

const start_text_gamma = ref("");
const key_gamma = ref("");
const result_gamma = ref("");

const finish_text_gamma = ref("");
const finish_key_gamma = ref("");
const finish_result_gamma = ref("");

const start_text = ref("");
const key = ref("");
const result = ref("");

const finish_text = ref("");
const finish_key = ref("");
const finish_result = ref("");

const data = ref({
  gamma: "",
});

const encryption = () => {
  result.value = encryptionFunc(start_text.value, key.value);
};

const decryption = () => {
  finish_result.value = decryptionFunc(finish_text.value, finish_key.value);
};

const encryption_gamma = () => {
  const gamma = generateRandomKey(start_text_gamma.value.length);
  key_gamma.value = gamma;
  result_gamma.value = encryptionGammaFunc(start_text_gamma.value, gamma);
  console.log(start_text_gamma.value);
};

const decryption_gamma = () => {
  finish_result_gamma.value = decryptionGammaFunc(
    finish_text_gamma.value,
    finish_key_gamma.value,
  );
};

const encryptionFunc = (start_text, key) => {
  start_text = start_text.toUpperCase();
  key = key.toUpperCase();

  var result = "";
  var keyIn = 0;

  for (var i = 0; i < start_text.length; i++) {
    var charCode = start_text.charCodeAt(i);

    if (charCode >= 65 && charCode <= 90) {
      var sh = key.charCodeAt(keyIn) - 65;
      var encryptedCharCode = ((charCode + sh - 65) % 26) + 65;
      result += String.fromCharCode(encryptedCharCode);
      keyIn = (keyIn + 1) % key.length;
    } else {
      result += start_text.charAt(i);
    }
  }
  // console.log("encryptionFunc", result);
  return result;
};

const decryptionFunc = (finish_text, finish_key) => {
  finish_text = finish_text.toUpperCase();
  finish_key = finish_key.toUpperCase();

  var result = "";
  var keyIn = 0;

  for (var i = 0; i < finish_text.length; i++) {
    var charCode = finish_text.charCodeAt(i);

    if (charCode >= 65 && charCode <= 90) {
      var sh = finish_key.charCodeAt(keyIn) - 65;
      var decryptCharCode = ((charCode - sh - 65 + 26) % 26) + 65;
      result += String.fromCharCode(decryptCharCode);
      keyIn = (keyIn + 1) % finish_key.length;
    } else {
      result += finish_text.charAt(i);
    }
  }

  return result;
};

const encryptionGammaFunc = (text, key) => {
  let result = "";

  for (let i = 0; i < text.length; i++) {
    const charCode = text.charCodeAt(i);
    const keyCh = key.charCodeAt(i % key.length);

    const encryptedCharCode = (charCode + keyCh) % 128;
    result += String.fromCharCode(encryptedCharCode);
  }

  return result;
};

const decryptionGammaFunc = (text, key) => {
  var result = "";

  for (var i = 0; i < text.length; i++) {
    const charCode = text.charCodeAt(i);
    const keyCh = key.charCodeAt(i % key.length);

    const decryptedCharCode = (charCode - keyCh + 128) % 128;
    result += String.fromCharCode(decryptedCharCode);
  }

  return result;
};

const generateRandomKey = (length) => {
  var key = "";
  const characters =
    "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

  for (var i = 0; i < length; i++) {
    const randomIndex = Math.floor(Math.random() * characters.length);
    key += characters.charAt(randomIndex);
  }

  return key;
};
</script>

<style scoped>
.shifr {
  padding: 15px;
}

.deshifr {
  padding: 15px;
}
</style>
