<template>
  <section>
    <!-- 목록에 애니메이션 추가 -->
    <transition-group name="list" tag="ul">
      <li
        v-for="(todoItem, index) in propsdata"
        class="shadow"
        v-bind:key="todoItem"
        @dblclick="popupForEdit(todoItem, index)"
      >
        <i class="checkBtn fa fa-check" aria-hidden="true"></i>
        {{ todoItem }}
        <span class="removeBtn" type="button" @click="removeTodo(todoItem, index)">
          <i class="far fa-trash-alt" aria-hidden="true"></i>
        </span>
      </li>
    </transition-group>
    <!-- modal -->
    <!-- <modal v-if="showModal" @close="showModal = false"> -->
    <modal v-if="showModal" @close="modalClose">
      <h3 slot="header">caution</h3>
      <div slot="body" class="inputBox shadow">
        <input type="text" v-model="editedItem" :placeholder="placeholderValue" v-on:keypress.enter="editTodo">
        <span class="addContainer" @click="editTodo">
          <i class="addBtn fas fa-plus"></i>
        </span>
      </div>
      <span slot="footer" @click="modalClose">
        todo item 변경사항을 입력하세요.
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </span>
    </modal>
  </section>
</template>

<script>
import Modal from './common/Modal.vue';

export default {
  data() {
    return {
        showModal: false,
        selectedItem: '',
        selectedIndex: '',
        editedItem: '',
        placeholderValue: ''
    }
  },
  props: ['propsdata'],
  methods: {
    removeTodo(todoItem, index) {
      //index는 vue에서 관리하고 제공하는 변수
      //console.log('todoItem, index: ' + todoItem, index);
      //선택목록을 로컬스토리지와 뷰 데이터에서 삭제
      // localStorage.removeItem(todoItem);
      // this.todoItems.splice(index, 1);
      //이벤트 전달 방식으로 개선
      this.$emit('removeTodo', todoItem, index);
    },
    popupForEdit(todoItem, index) {
      this.selectedItem = todoItem;
      this.selectedIndex = index;
      this.placeholderValue = this.selectedItem;
      this.showModal = !this.showModal;
    },
    editTodo() {
      if (this.editedItem !== "") {
        let editedValue = this.editedItem && this.editedItem.trim();
        this.$emit('editTodo', this.selectedItem, editedValue, this.selectedIndex);
        this.modalClose();
      } else {
        this.showModal = false;
      }
    },
    modalClose() {
      this.showModal = false;
      this.clearValues();
    },
    clearValues() {
      this.selectedItem = '';
      this.selectedIndex = '';
      this.placeholderValue = '';
    }
  },
  components: {
    Modal : Modal
  }
}
</script>

<style scoped>
  ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
  }
  li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
  }

  /* animation용 추가 */
  .list-item {
    display: inline-block;
    margin-right: 10px;
  }
  .list-move {
    transition: transform 1s;
  }
  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }
  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }

  .checkBtn {
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
  }
  .removeBtn {
    margin-left: auto;
    color: #de4343;
  }

  /* modal */
  input:focus {
    outline: none;
  }
  .inputBox {
    background: white;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
  }
  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }
  .addContainer {
    float: right;
    background: linear-gradient(to right, #6478fb, #8763fb);
    display: inline-block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
  }
  .addBtn {
    color: white;
    vertical-align: middle;
  }
</style>
