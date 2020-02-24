<template>
  <div id="app">
    <div class="row justify-center" style="margin-top:64px">
      <div class="col-6">
        <q-table
          title="Users"
          :columns="tblUserColumns"
          :data="tblUserData"
          :rows-per-page-options="tblUserRowsPerPageOptions"
          :pagination.sync="tblUserPagination"
          :loading="tblUserIsLoading"
          @request="tblUserOnRequest"
        >
          <template v-slot:body="props">
            <q-tr :props="props">
              <q-td :props="props" key="id">{{ props.row.id }}</q-td>
              <q-td :props="props" key="email">{{ props.row.email }}</q-td>
              <q-td :props="props" key="first_name">{{ props.row.first_name }}</q-td>
              <q-td :props="props" key="last_name">{{ props.row.last_name }}</q-td>
              <q-td :props="props" key="avatar">
                <q-avatar size="40px">
                  <img :src="props.row.avatar" />
                </q-avatar>
              </q-td>
            </q-tr>
          </template>
        </q-table>
      </div>
    </div>

    <br />
  </div>
</template>

<script>
export default {
  data() {
    return {
      // table User config
      tblUserColumns: [
        { name: "id", label: "ID", align: "left" },
        { name: "email", label: "Email", align: "left" },
        { name: "first_name", label: "First Name", align: "left" },
        { name: "last_name", label: "Last Name", align: "left" },
        { name: "avatar", label: "Avatar", align: "center" }
      ],
      tblUserRowsPerPageOptions: [3, 5, 10, 15, 25, 50, 100],
      tblUserPagination: {
        page: 1,
        rowsPerPage: 3,
        rowsNumber: 10
      },
      tblUserIsLoading: false,
      tblUserData: []
    };
  },
  mounted() {
    this.tblUserOnRequest({
      pagination: this.tblUserPagination,
      filter: undefined
    });
  },
  methods: {
    tblUserOnRequest(props) {
      this.tblUserIsLoading = true;

      fetch(
        "https://reqres.in/api/users?page=" +
          props.pagination.page +
          "&per_page=" +
          props.pagination.rowsPerPage
      ).then(result => {
        result.json().then(response => {
          this.tblUserIsLoading = false;

          this.tblUserData = response.data;
          this.tblUserPagination.page = props.pagination.page;
          this.tblUserPagination.rowsPerPage = props.pagination.rowsPerPage;
          this.tblUserPagination.rowsNumber = response.total;
        });
      });
    }
  }
};
</script>