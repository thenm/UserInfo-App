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
        <div class="message">
          <div class="message-body">
            <!-- <p class="help is-danger is-pulled-right">*All the field are </p> -->
            <form v-on:submit.prevent="addUsers">
              <div class="field">
                <label class="label has-text-grey">Name</label>
                <div class="control has-icons-left">
                  <input class="input" type="text" v-model="name" placeholder="Please enter your First and Last name" title="Only Alphabets" pattern="[a-zA-Z]+[ ][a-zA-Z]+"  required>
                  <span class="icon is-small is-left">
                    <i class="fa fa-user"></i>
                  </span>
                </div>
              </div>
              <div class="field">
                <label class="label has-text-grey">Email</label>
                <div class="control has-icons-left">
                  <input class="input" type="email" v-model="email" placeholder="Please enter your email address"  pattern="[^@\s]+@[^@\s]+\.[^@\s]+" title="Invalid email address" required>
                  <span class="icon is-small is-left">
                    <i class="fa fa-envelope"></i>
                  </span>
                </div>
              </div>
              <div class="field">
                <label class="label has-text-grey">Web Address</label>
                <div class="control has-icons-left">
                  <input class="input" type="url" v-model="web" placeholder="Please enter your web address" required>
                  <span class="icon is-small is-left">
                    <i class="fa fa-globe"></i>
                  </span>
                </div>
              </div>
              <div class="field">
                <label class="label has-text-grey">Cover Letter</label>
                <div class="control">
                  <textarea class="textarea" v-model="coverletter" placeholder="Please enter cover letter" required></textarea>
                </div>
              </div>

              <div class="field">
                <label class="label has-text-grey">Resume</label>
                <div class="control">
                  <textarea class="textarea" v-model="resume" placeholder="Please enter cover letter" required></textarea>
                  <!-- <input class="text-area" v-model="resume" type="file" name="resume" required> -->
                  <span class="file-name"></span>
                </div>
              </div>

              <div class="field">
                <label class="label has-text-grey">Do you like working?</label>
                <div class="control">
                  <label class="radio">
                    <input v-model="work" type="radio" name="worklove" value="Yes" required>
                    Yes
                  </label>
                  <label class="radio">
                    <input v-model="work" type="radio" name="worklove" value="No" required>
                    No
                  </label>
                </div>
              </div>

              <div class="field">
                <label class="label has-text-grey">Verify yourself: What is {{randomNumber1}} + {{randomNumber2}} ?</label>
              </div>

              <div class="field has-addons">
                <div class="control">
                  <input class="input" type="text" placeholder="enter sum" v-model="captchaInput" required>
                </div>
                <div class="control">
                  <a v-on:click="verifyUser" class="button is-primary">
                    <span v-if="!verificationCorrect">Verify</span>
                    <span class="icon" v-if="verificationCorrect">
                      <i class="fa fa-check" aria-hidden="true"></i>
                    </span>
                  </a>
                </div>
              </div>
              <p class="help is-danger" v-if="verificationHelp">Please enter correct sum </p>
              <div class="field">
                <div class="control has-text-right">
                  <button class="button is-primary" :disabled="isDisabled">Submit</button>
                </div>
              </div>
            </form>
          </div>
          <p class="help is-danger">*All the field are required</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CONSTANTS from '@/utils/constants'
import axios from 'axios'

export default {
  name: 'home',
  data: function () {
    return {
      randomNumber1: '',
      randomNumber2: '',
      captchaInput: '',
      sum: '',
      verificationCorrect: false,
      verificationHelp: false,
      isDisabled: true,
      name: '',
      email: '',
      web: '',
      coverletter: '',
      resume: '',
      work: ''
    }
  },
  created () {
    this.randomNums()
  },
  methods: {
    randomNums: function () {
      this.randomNumber1 = Math.floor(Math.random() * 100) + 1
      this.randomNumber2 = Math.floor(Math.random() * 100) + 1
      const sumOfRandomNumbers = this.randomNumber1 + this.randomNumber2
      // this.verifyUser(sumOfRandomNumbers)
      this.sum = sumOfRandomNumbers
    },

    verifyUser: function () {
      if (this.sum === Number(this.captchaInput)) {
        console.log('---------', this.captchaInput)
        this.verificationCorrect = true
        this.isDisabled = false
      } else {
        this.verificationHelp = true
      }
    },
    addUsers: function () {
      const user = {
        name: this.name,
        email: this.email,
        web: this.web,
        coverletter: this.coverletter,
        resume: this.resume,
        worklove: this.work
      }
      axios.post(`${CONSTANTS.API_URL}/api/users`, user)
      .then(response => {
        alert('User added')
      })
      .catch(e => {
        this.errors.push(e)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .hero-body {
    padding: 1.5rem 1.5rem;
  }

  .message {
    padding: 2.5rem 2rem;
  }
</style>
