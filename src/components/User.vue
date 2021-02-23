<template>
  <tr>
    <td>
      {{
      user.name.split(" ")[0] == "Mrs." || user.name.split(" ")[0] == "Mr."
      ? user.name.split(" ")[1]
      : user.name.split(" ")[0]
      }}
    </td>
    <td>
      {{
      user.name.split(" ")[0] == "Mrs." || user.name.split(" ")[0] == "Mr."
      ? user.name.split(" ")[2]
      : user.name.split(" ")[1]
      }}
    </td>
    <td>{{ user.email }}</td>
    <td>{{ user.address.city }}</td>
    <td>{{ this.country.countryName }}</td>
    <td>{{ user.company.name }}</td>
    <td>{{ user.phone }}</td>
  </tr>
</template>

<script>
export default {
  data() {
    return {
      country: null
    };
  },
  props: {
    user: Object
  },
  created() {
    fetch(
      `http://api.geonames.org/countryCode?lat=${parseFloat(
        this.user.address.geo.lat
      )}&lng=${parseFloat(
        this.user.address.geo.lng
      )}&type=JSON&username=pepster98`
    )
      .then(response => response.json())
      .then(json => {
        this.country = json;
      });
  }
};
</script>
