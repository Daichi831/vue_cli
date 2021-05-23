<template>
  <div class="app" id="app">
    <h1>Memo List</h1>
    <ul>
      <li v-for="(memo, index) in memos" v-bind:key="index">
        <a v-on:click="edit(index)">{{ memo.split('\n')[0] }}</a>
      </li>
    </ul>
    <div class="plus" v-on:click="show()">+</div>
    
    <div v-if="this.showPage">
      <div class="show" v-if="!this.editIndex">
        <h2>New Memo</h2>
        <textarea v-model="newMemo"></textarea>
        <button class="b-add" v-on:click="add()">追加</button>
      </div>
    </div>

    <div class="show" v-if="this.editIndex">
      <h2>Show Memo</h2>
      <textarea v-model="editMemo"></textarea>
      <button class="b-update" v-on:click="update(editIndex)">更新</button>
      <button class="b-remove" v-on:click="remove(editIndex)">削除</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      memos: [],
      newMemo: null,
      editMemo: null,
      editIndex: null,
      showPage: false
    };
  },
  mounted() {
    if (localStorage.getItem("memos")) {
      this.memos = JSON.parse(localStorage.memos);
    }
  },
  methods: {
    show: function() {
      if (this.showPage === false) {
        this.showPage = true;
      } else {
        this.showPage = false;
      }
      this.newMemo = "";
    },
    add: function() {
      if (!this.newMemo) {
        return;
      }
      this.memos.push(this.newMemo);
      this.newMemo = "";
      this.showPage = false;
      this.save();
    },
    remove: function(index) {
      this.memos.splice(index - 1, 1);
      this.newMemo = "";
      this.editIndex = null;
      this.save();
    },
    edit: function(index) {
      this.editMemo = this.memos[index];
      this.editIndex = index + 1;
      if (this.showPage === true) {
        this.showPage = false;
      }
    },
    update: function(index) {
      if (!this.editMemo) {
        return;
      }
      this.memos[index - 1] = this.editMemo;
      this.editMemo = "";
      this.editIndex = null;
      this.save();
    },
    save: function() {
      localStorage.setItem("memos", JSON.stringify(this.memos));
    }
  }
};
</script>

<style>
textarea {
  resize: none;
  width: 200px;
  height: 250px;
  font-size: 1.5em;
}

.show {
  position: relative;
}

.b-add {
  position: absolute;
  width: 100px;
  bottom: -25px;
  left: 50px;
}

.b-remove {
  position: absolute;
  width: 100px;
  bottom: -25px;
  left: 105px;
}

.b-update {
  position: absolute;
  width: 100px;
  bottom: -25px;
  left: 0px;
}

.plus {
  font-size: 25px;
}

</style>
