<script>
const canvas = [];
for (let i = 0; i < 43; i++) {
  canvas[i] = [];
  for (let j = 0; j < 70; j++) {
    canvas[i].push({
      id: i + ":" + j,
      color: "transparent",
    });
  }
}
export default {
  name: 'px-canvas',
  data() {
    return {
      canvas,
      clicked: false,
      removeBorder: false,
      bucketCheck: false,
      colorPicker: {
        selected: 0, 
      },
      colors: {
        0: '#74b9ff',
        1: '#00cec9',
        2: '#6c5ce7',
        3: '#d63031',
        4: '#dfe6e9',
        5: '#000000', 
      },
    };
  },
  methods: {
    changeColor(id, options) {
      if (this.clicked) {
        const str = [...id.split(":")];
        this.canvas[str[0]][str[1]].color = this.colors[this.colorPicker.selected];
      }
    },
    changeColorAll() {
      for (line in this.canvas) {
        for (col in this.canvas[line]) {
          console.log(col);
        }
      }
    },
    mousedown(id) {
      this.clicked = true;
      this.changeColor(id, { click: true });
    },
    selectedColor(type){
      this.colorPicker.selected = type
    }
  },
};
</script>

<template lang="pug">
main
  .px-canvas(v-for="(line , index) in canvas")
    .cell.border(v-for="(item , index2) in canvas[index]"
        @mouseenter="changeColor(item.id)"
        @mousedown="mousedown(item.id)"
        @mouseup="clicked = false"
        :style="{ backgroundColor: item.color }"
        :class="{'border-transparent': removeBorder}")
  .sidebar
    .color-container(v-for="(cls , index) in colors")
      .color-picker(@click="selectedColor(index)" :style="{backgroundColor: colors[index]}" :class='{"active": colorPicker.selected == index}')
      label(:for="'color-' + index")
        input(type="color" v-model="colors[index]" :id="'color-' + index")
        i.fa-sharp.fa-solid.fa-caret-down
    label.border-checkbox(for="remove-borders")
      input(id="remove-borders" type="checkbox" v-model="removeBorder")
      a.px-btn
        | border
</template>


<style lang="scss" scoped>
.px-canvas {
  margin: 0 auto;
  width: 1050px;
  display: flex;
  flex-wrap: wrap;
  user-select: none;
}
.cell {
  width: 15px;
  height: 15px;
  background: transparent;
  user-select: none;
  box-sizing: border-box
}
.cell.border {
  border: 0.5px solid #202020;
}
.cell.border.border-transparent {
  border: 0.5px solid transparent;
}
.color-container {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  i {
    margin-top: 7px;
  }
}
.color-picker {
  width: 30px;
  height: 30px;
  background: none;
  border-radius: 50%;
  border: 2px solid whitesmoke;
  outline: 2px solid transparent;
  margin-bottom: 15px;
  cursor: pointer;
  &.active  , &:hover {
    box-shadow: 0px 0px 0px 6px whitesmoke;
    outline: 4px solid black;
  }
}
input[type="color"] { display: none }
.sidebar {
  position: fixed;
  top: 0;
  right: 0;
  width: 80px;
  height: 100vh;
  border-left: 2px solid #202020;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items:center;
}
.border-checkbox {
  input {
    display: none;
  }
}
</style>