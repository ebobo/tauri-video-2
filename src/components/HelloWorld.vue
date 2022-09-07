<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <v-img :src="logo" class="my-3" contain height="200" />
      </v-col>

      <v-col class="mb-4">
        <h1 class="display-2 font-weight-bold mb-3">
          Tauri + Vite + Vue3 + Vuetify3 + TypeScript
        </h1>

        <h4>Video Player Testing</h4>

        <p class="subheading font-weight-regular">
          For display video streaming
        </p>
      </v-col>
    </v-row>
    <v-row class="text-center">
      <video width="500" muted loop autoplay controls>
        <source src="../assets/camera1.webm" type="video/webm" />
        Your browser does not support the video tag.
      </video>
    </v-row>
    <v-row>
      <v-text-field
        class="text-editor"
        v-model="object.text"
        variant="underlined"
        color="#82b1ff"
      ></v-text-field>
      <v-btn class="button" @click="writeConfig">Write</v-btn>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import {
  createDir,
  readTextFile,
  writeTextFile,
  BaseDirectory,
} from '@tauri-apps/api/fs';

// Logo
import logo from '../assets/logo.svg';

export default defineComponent({
  name: 'HelloWorld',

  data() {
    return {
      logo,
      object: { text: 'some default text' },
    };
  },
  created() {
    this.readConfig();
  },

  methods: {
    async readConfig() {
      try {
        const contents = await readTextFile('settings/app.json', {
          dir: BaseDirectory.App,
        });
        const obj = JSON.parse(contents);
        console.log(obj.text);
        this.object.text = obj.text;
      } catch (e) {
        await createDir('settings', {
          dir: BaseDirectory.App,
          recursive: true,
        });
      }
    },

    async writeConfig() {
      const stringJSON = JSON.stringify(this.object);
      await writeTextFile(
        { path: 'settings/app.json', contents: stringJSON },
        { dir: BaseDirectory.App }
      );
    },
  },
});
</script>

<style lang="scss" scoped>
.text-editor {
  margin-top: 1rem;
  width: 100px;
  height: 60px;
}

.text-editor input {
  text-align: center;
}

.button {
  margin-top: 1.5rem;
  margin-left: 1rem;
}
</style>
