<template>
  <v-layout>
    <v-flex
      text-xs-center
      xs12
      sm6
      offset-sm3
    >
      <img
        :src="require('@/assets/Image/cash.png')"
        alt="logo"
        width="50px"
        height="50px"
      >
      <h3 class="mb-4">I love money</h3>
      <v-text-field
        prepend-icon="person"
        name="login"
        label="Login"
        type="text"
        v-model="formEmail"
      ></v-text-field>
      <v-text-field
        prepend-icon="lock"
        name="password"
        label="Password"
        id="password"
        type="password"
        v-model="formPassword"
      ></v-text-field>
      <v-btn
        class="signIn m-t-10 w-200"
        @click="emailLogin"
        :loading="loading"
        primary
      >Đăng nhập</v-btn>
      <v-layout
        class="m-tb-10"
        justify-space-between
      >
        <span
          class="red--text"
          @click="forgotPasswordDialog = true"
          style="cursor:pointer"
        >Quên mật khẩu</span>
        <router-link to="/signup">Đăng ký</router-link>
      </v-layout>
      <span>________OR________</span>
      <v-layout
        justify-center
        column
        align-center
      >
        <v-btn
          style="color:white;width: 250px;"
          color="#DB4437"
          class=""
          primary
          @click.native="googleSignUp"
        >
          <v-icon>fa-facebook-f</v-icon>Đăng nhập với Google
        </v-btn>
        <v-btn
          style="color:white;width: 250px;"
          color="#3C5A99"
          class=""
          primary
          @click.native="facebookSignUp"
        >
          <v-icon>fas fa-facebook</v-icon>Đăng nhập với Facebook
        </v-btn>
      </v-layout>
    </v-flex>
    <v-dialog
      v-model="forgotPasswordDialog"
      max-width="600px"
    >
      <v-card>
        <v-card-title>
          <span class="headline pb-1">Quên mật khẩu</span>
          Hãy nhập email mà bạn đã dùng để đăng ký. Chúng tôi sẽ gửi cho bạn một email để đặt lại mật khẩu của bạn
        </v-card-title>
        <v-card-text style="padding-top:0">
          <v-form ref="forgetAccountForm">
            <v-layout wrap>
              <v-flex
                xs12
                sm12
                md12
              >
                <v-text-field
                  style="padding-top:0"
                  label="Email"
                  type="email"
                  v-model="emailreset"
                ></v-text-field>
              </v-flex>
            </v-layout>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <!-- <v-btn color="blue darken-1" flat @click="forgotPasswordDialog = false">Đóng</v-btn> -->
          <v-btn
            color="blue darken-1"
            flat
            @click="resetPassword"
          >Gửi email đặt lại mật khẩu</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-layout>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  data() {
    return {
      loading: false,
      formEmail: '',
      formPassword: '',
      dialog: false,
      emailreset: '',
      forgotPasswordDialog: false
    }
  },
  methods: {
    // snackTime: function(snack, color) {
    //   this.setSnack({ msg: 'awdawdw', color: 'warning' })
    //   // this.$router.push('/')
    // },
    ...mapMutations({
      setSnack: 'snackbar/setSnack'
    }),
    emailLogin() {
      this.loading = true
      this.$store
        .dispatch('user/signInWithEmail', {
          email: this.formEmail,
          password: this.formPassword
        })
        .then(() => {
          this.formEmail = ''
          this.formPassword = ''
          this.setSnack({ msg: 'Đăng nhập thành công', color: 'success' })
          this.loading = false
        })
        .catch(data => {
          this.formEmail = ''
          this.formPassword = ''
          this.setSnack({ msg: data.message, color: 'error' })
          this.loading = false
        })
    },
    googleSignUp() {
      this.$store
        .dispatch('user/signInWithGoogle')
        .then(() => {
        })
        .catch(e => {
          this.setSnack({ msg: e.message, color: 'error' })
        })
    },
    facebookSignUp() {
      this.$store
        .dispatch('user/signInWithFacebook')
        .then(() => {
        })
        .catch(e => {
          this.setSnack({ msg: e.message, color: 'error' })
        })
    },
    resetPassword() {
      if (this.emailreset.length > 0) {
        const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
        return this.$store
          .dispatch('user/resetPassword', {
            email: this.emailreset
          })
          .then(() => {
            this.emailreset = ''
            this.setSnack({
              msg: 'Một email xác nhận đã được gửi đến mail của bạn',
              color: 'success'
            })
            this.forgotPasswordDialog = false
          })
          .catch(data => {
            this.emailreset = ''
            this.setSnack({ msg: data.message, color: 'error' })
          })
      } else
        this.setSnack({
          msg: 'Vui lòng không bỏ trống',
          color: 'error'
        })
    }
  }
}
</script>

<style lang="css">
</style>
