<template>
  <div class="home">
    <ul class="container">
      <li
        v-for="item in data"
        :key="item.code"
        class="box-item"
        draggable
        :ref="`li-${item.code}`"
      >
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
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
interface FileItem {
  name: string;
  code: number;
}
@Component({
  components: {
    HelloWorld,
  },
})
export default class HomeView extends Vue {
  data: FileItem[] = [
    { name: "abc", code: 1 },
    { name: "sahou", code: 2 },
    { name: "gfabguoh", code: 3 },
    { name: "asdxxz", code: 4 },
    { name: "asxccv", code: 5 },
    { name: "sfaxxxz", code: 6 },
  ];

  handleDragStart(ev: DragEvent) {
    console.log(ev);
    console.log("start");
    if (ev.dataTransfer) {
      // Add the target element's id to the data transfer object
      ev.dataTransfer.dropEffect = "move";
    }
  }
  handleDragEnd(ev: DragEvent) {
    console.log(ev);
    console.log("end");
    if (ev.dataTransfer) {
      // Add the target element's id to the data transfer object
      ev.dataTransfer.dropEffect = "move";
    }
  }
  handleDragOver(ev: DragEvent) {
    console.log(ev);
    console.log("over");
    let targetEl = ev.target as HTMLElement;
    if (ev.dataTransfer) {
      // Add the target element's id to the data transfer object
      ev.dataTransfer.dropEffect = "move";
    }
  }
  mounted() {
    console.log("iam");
    if (this.$refs) {
      console.log("yes");
      for (let item of this.data) {
        console.log(this.$refs[`li-${item.code}`]);
        let el = this.$refs[`li-${item.code}`][0] as Element;
        el.addEventListener("dragstart", (event as DragEvent) =>
          this.handleDragStart(event,item)
        );
        el.addEventListener("dragover", this.handleDragOver);
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
