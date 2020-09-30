<template>
  <div
    class="wrapper"
    @dragover.prevent="dropper = true"
    @dragleave="dropper = false"
    @drop.prevent
  >
    <button @click="hey">download ZIP</button>

    <div
      class="drop"
      :class="{'dragover': dragover, 'dropper': dropper}"
      @dragover.prevent="dragover = true"
      @dragleave="dragover = false"
      @drop.prevent="onDrop"
    >
      ここにドロップ
      <label>
        ファイルを選択
        <input
          type="file"
          style="display: none;"
          class="file_input"
          name="photo"
          @change="onFileChange"
        />
      </label>
    </div>

    <ul>
      <li v-for="item in result" :key="item">{{item}}</li>
    </ul>
  </div>
</template>
<style>
.drop {
  background: gray;
  padding: 4rem;
}
label {
  display: inline-block;
  border: 1px solid black;
  background: white;
}
.dragover {
  background: yellow;
}
.dropper {
  height: 20rem;
}
</style>
<script>
import { saveAs } from "file-saver";
import { SourceMap } from "module";
const JSZip = require("jszip");

export default {
  data() {
    return {
      dragover: false,
      result: [],
      dropper: false
    };
  },
  methods: {
    onDrop(e) {
      const files = event.dataTransfer.files;
      // if (files.length !== 1 || files[0].type.indexOf("image") !== 0) {
      //   return;
      // }
      this.loadZip(files[0]);
      this.dragover = false;
      this.dropper = false;
    },
    onFileChange(e) {
      let files = e.target.files || e.dataTransfer.files;
      // console.log(files[0]);
      this.loadZip(files[0]);
      // this.createImage(files[0]);
    },
    loadZip(f) {
      this.result = [];

      JSZip.loadAsync(f).then(
        zip => {
          zip.forEach((relativePath, zipEntry) => {
            console.log(zipEntry);
            this.result.push(zipEntry.name);
          });
        },
        e => {
          console.log(e);
        }
      );
    },
    hey() {
      const imgData = "";

      var zip = new JSZip();
      zip.file("Hello.txt", "Hello World\n");
      var img = zip.folder("images");
      img.file("smile.gif", imgData, { base64: true });
      zip.generateAsync({ type: "blob" }).then(function(content) {
        // see FileSaver.js
        saveAs(content, "example.zip");
      });
    }
  }
};
</script>