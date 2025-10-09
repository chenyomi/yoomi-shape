<script setup lang="ts">
import { onMounted, computed, ref, watchEffect } from "vue";
import { App, Rect, Text, Group, PointerEvent } from "leafer-ui";
import "leafer-editor";
import "@leafer-in/state";
import { graphic, line } from "./data";
const uniquegGraphic = computed(() => {
  const map: any = [];
  for (const item of graphic.children) {
    if (!map.includes(item.name)) {
      map.push(item.name);
    } else {
      (item as any).name = null;
    }
  }
  return [...graphic.children];
});
let app: any = null
onMounted(() => {
  app = new App({ view: "leafer-view", fill: "#ffffff", editor: {
    selector: true,  // 禁用选择器  
  } });
  let { width = 1080, height = 960 } = app;

  const vue = new Rect({
    width: 100,
    height: 100,
    fill: {
      type: "image",
      url: "/vue.svg",
      mode: "fit",
    },
    editable: true,
    hoverStyle: {
      shadow: {
        x: 0,
        y: 0,
        blur: 20,
        color: "#42b883aa",
      },
    },
  });
  const vite = new Rect({
    x: 150,
    width: 100,
    height: 100,
    fill: {
      type: "image",
      url: "/vite.svg",
      mode: "fit",
    },
    hoverStyle: {
      shadow: {
        x: 0,
        y: 0,
        blur: 20,
        color: "#646cffaa",
      },
    },
    editable: true,
  });
  const leaferJS = new Rect({
    x: 300,
    width: 100,
    height: 100,
    fill: {
      type: "image",
      url: "/leafer.svg",
      mode: "fit",
    },
    hoverStyle: {
      shadow: {
        x: 0,
        y: 0,
        blur: 20,
        color: "#32cd79",
      },
    },
    editable: true,
  });
  leaferJS.on(PointerEvent.DOUBLE_TAP, () => {
    window.open("https://www.leaferjs.com/ui/guide/");
  });
  let logoGroup = new Group({
    x: width / 2 - 200,
    y: height * 0.4,
    children: [vue, vite, leaferJS],
  });
  app.tree.add(logoGroup);
});
const currentSelect = ref(null);
const onClick = (item: any) => {
  if (item.json.name == currentSelect.value) {
    currentSelect.value = null;
    app.editor.config.selector = true
  } else {
    currentSelect.value = item.json.name;
    app.editor.config.selector = false
    console.log(app)
    
  }
};
</script>

<template>
  <div class="left-panel">
    <img
      v-for="item in uniquegGraphic"
      :class="currentSelect == item.json.name && 'active'"
      :src="`/assets/shape/${item.icon.split('-')[2]}.svg`"
      alt=""
      @click="onClick(item)"
    />
  </div>
  <div id="leafer-view"></div>
</template>

<style scoped>
#leafer-view {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.left-panel {
  position: absolute;
  left: 20px;
  top: 20px;
  z-index: 1;
  width: 250px;
  text-align: center;
}
.left-panel img {
  height: 40px;
  width: 40px;
  padding: 5px;
  margin: 5px;
  border-radius: 5px;
  border: 1px solid #ffffff;
  transition: all 0.2s ease-in-out;
}
.left-panel img:hover,
.left-panel .active {
  box-shadow: 0px 0px 3px #000000;
  border: 1px solid #000000;
  cursor: pointer;
}
</style>
