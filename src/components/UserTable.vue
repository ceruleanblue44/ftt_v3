<template>
<div>
     <!-- Not sure, but maybe it's a better idea to make the sort section a separate component? -->
      <div class="sort-section">
        <div>Сортировка:</div>
        <div :class="sortClass('registration_date')"
            @click = "sort('registration_date')"        
             > <div class="underline">Дата регистрации</div>  </div>
        <div :class="sortClass('rating')"
            @click = "sort('rating')"     
             ><div class="underline">Рейтинг</div></div>
      </div>

      <div class="table-section">
      <table>
        <thead>
        <tr>
          <th>Имя пользователя</th>
          <th>E-mail</th>
          <th>Дата регистрации</th>
          <th>Рейтинг</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in paginatedData"
            :key="user.id"
           
            >
          <td :style="styleObj"> {{ user.username }} </td>
          <td> {{ user.email }} </td>
          <td> {{ processDate(user.registration_date) }} </td>
          <td> {{ user.rating }} </td>
         
          <td class="x-del" @click="getId(user.id); passUserId();">  </td>

        </tr>
      </tbody>
      </table>
      <div class="modal" v-if="showModal">
        <div class="modal-msg">
          <p>Вы уверены, что хотите удалить пользователя?</p>
          <button type="button" @click="deleteUser(userId); closeModal(); ">Да</button>
          <button type="button" @click="closeModal()">Нет</button>
        </div>
      </div>
    </div>
</div>
</template>

<script>
export default {
  props: {
    userData: Array,
    styleObj: Object,
    sort: Function,
    isSorted: Boolean,
    currentSortDir: String,
    getPageItems: Function,
    paginatedData: Array,
    deleteUser: Function,
    sortClass: Function,
  },

  data() {
    return {
      userId: '',
      showModal: false,
    }
  },

  methods: {
    getId(id) {
      this.userId = id;
      this.showModal = true;
    },

    openModal() {
      this.showModal = false;
    },

    closeModal() {
      this.showModal = false;
    },

    passUserId: function () {
      this.$emit('passId', this.userId)
    },

    processDate(isoDate) {
      return isoDate.split('T')[0]
                        .split('-')
                        .reverse()
                        .toString()
                        .replace(/,/g, '.');
    },
  }
}
</script>
