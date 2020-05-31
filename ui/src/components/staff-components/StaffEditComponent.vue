<template>
  <div class="container">
    <form>
      <div class="row">
        <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-ms-offset-3">
          <h1>
            Persons page
          </h1>
          <div class="form-group">
            <label for="name">
              Name
            </label>
            <input id="name"
                   v-model="staff.name"
                   type="text"
                   class="form-control">
          </div>
          <div class="form-group">
            <label for="name">
              Surname
            </label>
            <input id="second_name"
                   v-model="staff.surname"
                   type="text"
                   class="form-control">
          </div>
          <div class="form-group">
            <label for="name">
              Position
            </label>
            <b-form-select v-model="staff.positionId"
                           class="mb-3">
              <b-form-select-option v-for="position in positions"
                                    :key="position.id"
                                    :value="position.id">
                {{ position.name }}
              </b-form-select-option>
            </b-form-select>
          </div>
          <b-button class="btn-dark"
                    @click="cancelMethod">
            Cancel
          </b-button>
          <b-button class="btn-dark"
                    @click="putMethod">
            Save
          </b-button>
        </div>

      </div>
    </form>
  </div>
</template>

<script>
  import axios from 'axios';
  import {baseUrl} from '../../utils/settings';

  export default {
    name: 'StaffMemberPage',
    props: {
      id: {
        type: String,
        required: true,
      },
    },
    data: () => (
      {
        staff: {
          id: '',
          name: '',
          surname: '',
          position: '',
          positionId:'',
        },
        e: null,
        positions: [],
      }),
    created () {
      axios.get(`${baseUrl}/staff/${this.id}`)
        .then(response => (this.staff = response.data))
        .catch(e => (this.error = e));
      axios.get(`${baseUrl}/positions`)
        .then(response => (this.positions = response.data))
        .catch(e => (this.error = e));
    },
    methods: {
      putMethod () {
        axios.put(`${baseUrl}/staff/${this.id}`, this.staff)
          .then(() => (this.$router.push('/main/staff')));
      },
      cancelMethod () {
        this.$router.push('/main/staff');
      },
    },
  };
</script>

<style scoped>

</style>
