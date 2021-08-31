<template>
  <div>
    <h1>Users Table</h1>
    <label for="users"><h3>Choose User :</h3></label>
    <select @change="handleChange" v-model="selectedUser">
      <option value="v-all">View All</option>
      <option
        :key="user.id"
        v-for="user in this.users"
        :value="user.first_name"
      >
        {{ user.first_name + " " + user.last_name }}
      </option>
    </select>
  </div>
  <hr />
  <table border="3px">
    <tr>
      <th>First Name</th>
      <th>Last Name</th>
      <th>Email</th>
    </tr>
    <tr :key="user.id" v-for="user in this.filteredUsers">
      <th>{{ user.first_name }}</th>
      <th>{{ user.last_name }}</th>
      <th>{{ user.email }}</th>
    </tr>
  </table>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      users: {},
      filteredUsers: {},
      selectedUser: null,
    };
  },
  mounted() {
    this.fetchUsers();
  },
  updated() {
    console.log(this.selectedUser);
  },
  methods: {
    async fetchUsers() {
      const response = await fetch("https://reqres.in/api/users");
      const json = await response.json();
      this.users = json.data;
      this.filteredUsers = this.users;
    },
    handleChange() {
      if (this.selectedUser == "v-all") {
        this.filteredUsers = this.users;
      } else if (this.selectedUser != null) {
        this.filteredUsers = this.users.filter((user) => {
          return user.first_name == this.selectedUser;
        });
      }
    },
  },
};
</script>

<style>
</style>
