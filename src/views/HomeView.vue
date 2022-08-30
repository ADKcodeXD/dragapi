<template>
  <div class="home">
    <ul class="container">
      <li v-for="item in data" :key="item.code" class="box-item" draggable :ref="`li-${item.code}`"
        :class="{ 'over-border': item.isHover }">
        <div>
          <p>13456</p>
          <span>shaouifhao</span>
        </div>
        <div>
          <h3>{{ item.name }}</h3>
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
interface FileItem {
  name: string;
  code: number;
  isHover: boolean;
}
@Component
export default class HomeView extends Vue {
  data: FileItem[] = [
    { name: "abc", code: 1, isHover: false },
    { name: "sahou", code: 2, isHover: false },
    { name: "gfabguoh", code: 3, isHover: false },
    { name: "asdxxz", code: 4, isHover: false },
    { name: "asxccv", code: 5, isHover: false },
    { name: "sfaxxxz", code: 6, isHover: false },
  ];
  private lastEnterNode!: Element;
  private currentElment!: FileItem | null;
  handleDragStart(ev: any, item: FileItem) {
    if (ev.dataTransfer) {
      // Add the target element's id to the data transfer object
      ev.dataTransfer.dropEffect = "move";
      this.currentElment = item
      console.log(this.currentElment);
    }
  }
  handleDragEnd(ev: DragEvent) {
    if (ev.dataTransfer) {
      // Add the target element's id to the data transfer object
      ev.dataTransfer.dropEffect = "move";
      this.currentElment = null
    }
  }
  handleDragEnter(ev: any, item: FileItem) {
    this.lastEnterNode = ev.target
    if (ev.dataTransfer) {
      if (this.currentElment && this.currentElment.code !== item.code) {
        item.isHover = true
        let index = this.data.findIndex(el => el.code === item.code)
        this.data.splice(index, 1, item)
      }
      ev.dataTransfer.dropEffect = "copy";
    }
  }
  handleDragLeave(ev: any, item: FileItem) {
    if (this.lastEnterNode === ev.target) {
      if (ev.dataTransfer) {
        let currentNode = ev.dataTransfer.getData('file')
        item.isHover = false
        let index = this.data.findIndex(el => el.code === item.code)
        this.data.splice(index, 1, item)
        ev.dataTransfer.dropEffect = "copy";
      }
    }
  }
  mounted() {
    if (this.$refs) {
      for (let item of this.data) {
        console.log(this.$refs[`li-${item.code}`]);
        let el = this.$refs[`li-${item.code}`][0] as Element;
        el.addEventListener("dragstart", (e) => this.handleDragStart(e, item));
        el.addEventListener("dragenter", (e) => this.handleDragEnter(e, item));
        el.addEventListener("dragleave", (e) => this.handleDragLeave(e, item));
        el.addEventListener("dragend", this.handleDragEnd);
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  display: flex;
  flex-wrap: wrap;
}

.box-item {
  width: 300px;
  height: 300px;
  margin: 20px;
  background-color: blueviolet;
  list-style: none;
}
.over-border {
  outline: green 3px dashed;
}
</style>
