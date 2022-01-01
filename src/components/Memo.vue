<template>
  <section class="alert alert-primary">
    <ul class="list-group">
      <li v-for="(memo, index, key) in memos" :key="key" @click="displayEditForm(index)" class="list-group-item list-group-item-action text-left btn">
        <span>{{ memo.split('\n')[0] }}</span>
      </li>
    </ul>
<!--    addFlagとeditFlagともにfalseの時-->
    <template v-if="!addFlag && !editFlag">
      <span class="btn btn-info m-2" @click="displayAddForm">＋</span>
    </template>
    <template v-else-if="addFlag">
      <textarea class="mt-5" rows="5" cols="50" v-model="newMemo"></textarea>
      <div>
        <button class="btn btn-info m-2" @click="addMemo">Add</button>
        <transition name="del">
          <button class="btn btn-info m-2" @click="cancel">Cancel</button>
        </transition>
      </div>
    </template>
    <template v-if="editFlag">
      <textarea class="mt-5" rows="5" cols="50" v-model="newMemo"></textarea>
      <div>
        <button class="btn btn-info m-2" @click="editMemo">Update</button>
        <button class="btn btn-info m-2" @click="deleteMemo">Delete</button>
        <transition name="del">
          <button class="btn btn-info m-2" @click="cancel">Cancel</button>
        </transition>
      </div>
    </template>
  </section>
</template>

<script>
export default {
  data () {
    return {
      newMemo: '',
      memos: [],
      addFlag: false,
      editFlag: false,
      editIndex: null
    }
  },
  methods: {
    displayAddForm () {
      this.addFlag = true
      this.editFlag = false
      this.newMemo = ''
    },
    cancel () {
      this.addFlag = false
      this.editFlag = false
      this.newMemo = ''
    },
    addMemo () {
      this.memos.push(this.newMemo)
      this.saveMemo()
    },
    displayEditForm (index) {
      this.editIndex = index
      this.addFlag = false
      this.editFlag = true
      this.newMemo = this.memos[this.editIndex]
    },
    editMemo () {
      this.memos.splice(this.editIndex, 1, this.newMemo)
      this.saveMemo()
    },
    deleteMemo () {
      if (confirm('Are you sure?')) {
        this.memos.splice(this.editIndex, 1)
        this.saveMemo()
      }
    },
    saveMemo () {
      localStorage.setItem('memos', JSON.stringify(this.memos))
      this.cancel()
    }
  },
  mounted () {
    this.memos = JSON.parse(localStorage.getItem('memos')) || [];
  },
}
</script>
