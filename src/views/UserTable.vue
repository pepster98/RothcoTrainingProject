<template>
  <div class="users">
    <h1>Users</h1>
    <input type="radio" id="name" value="name" v-model="picked" />
    <label for="name">Name</label>
    <input type="radio" id="phone" value="phone" v-model="picked" />
    <label for="phone">Phone</label>
    <input type="radio" id="city" value="city" v-model="picked" />
    <label for="city">City</label>
    <br />
    <input type="text" v-model="search" placeholder="search users" />
    <br />
    <button v-on:click="sortByCompany">Sort By Company</button>
    <table style="width:100%">
      <tr>
        <th>Firstname</th>
        <th>Lastname</th>
        <th>Email</th>
        <th>City</th>
        <th>Country</th>
        <th>Company</th>
        <th>Phone</th>
      </tr>
      <User v-for="user in filteredUsers" :key="user.id" :user="user" />
      <br />
    </table>
  </div>
</template>

<script>
import User from "@/components/User.vue";
export default {
  name: "UserTable",
  components: {
    User
  },
  data() {
    return {
      users: [],
      search: "",
      picked: "name"
    };
  },
  methods: {
    sortByCompany() {
      // sort By Country
      this.users.sort(function(a, b) {
        if (a.company.name > b.company.name) {
          return 1;
        } else if (a.company.name < b.company.name) {
          return -1;
        } else {
          return 0;
        }
      });
    }
  },
  created() {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(response => response.json())
      .then(json => {
        this.users = json;
      });
  },
  computed: {
    filteredUsers: function() {
      return this.users.filter(user => {
        if (this.picked === "name") {
          return user.name.match(this.search);
        } else if (this.picked === "phone") {
          return user.phone.match(this.search);
        } else if (this.picked === "city") {
          return user.address.city.match(this.search);
        }
      });
    }
  }
};
</script>
