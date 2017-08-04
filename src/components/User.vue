  <template>
 <div>
   <section class="hero is-primary">
     <div class="hero-body">
       <div class="container">
         <h1 class="title is-4 has-text-centered">
           User Information
         </h1>
       </div>
     </div>
   </section>
   <div class="columns is-centered is-mobile">
    <div class="column is-half">
      <table class="table is-bordered">
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Web</th>
            <th>Cover Letter</th>
            <th>Resume</th>
            <th>Love Work?</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="user in users">
            <td>{{user.name}}</td>
            <td>{{user.email}}</td>
            <td>{{user.web}}</td>
            <td>{{user.coverletter}}</td>
            <td>{{user.resume}}</td>
            <td>{{user.worklove}}</td>
          </tr>
        </tbody>
      </table>
    </div>
   </div>
</div>

</template>
<script>
import axios from 'axios'
import CONSTANTS from '@/utils/constants'
export default {
  name: 'User',
  data () {
    return {
      users: [],
      errors: []
    }
  },
  created () {
    console.log('indside created')
    this.getUser()
  },
  methods: {
    getUser: function () {
      console.log('inside function')
      axios.get(`${CONSTANTS.API_URL}/api/users`)
      .then(response => {
         // JSON responses are automatically parsed.
        this.users = response.data
        console.log(this.users.body)
      })
     .catch(e => {
       this.errors.push(e)
     })
    }
  }
}
</script>
<style scoped>
.hero-body {
  padding: 1.5rem 1.5rem;
}

.message {
  padding: 2.5rem 2rem;
}
</style>
