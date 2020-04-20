<template>
  <div class="bg">
    <template>
      <v-form ref="form" :model="validateForm" class="mu-demo-form">
        <v-text-field v-model="validateForm.username" :rules="usernameRules" label="用户名" prop="username"></v-text-field>

        <v-text-field v-model="validateForm.password" :rules="passwordRules" label="密码" prop="password"></v-text-field>

        <v-checkbox v-model="validateForm.isAgree" label="同意用户协议"  prop="isAgree" :rules="argeeRules"></v-checkbox>

        <v-btn class="mr-4" @click="submit">
          提交
        </v-btn>

        <v-btn color="error" class="mr-4" @click="clear">
          重置
        </v-btn>
      </v-form>
    </template>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      usernameRules: [
        { validate: (val) => !!val, message: '必须填写用户名' },
        { validate: (val) => val.length >= 3, message: '用户名长度大于3' }
      ],
      passwordRules: [
        { validate: (val) => !!val, message: '必须填写密码' },
        { validate: (val) => val.length >= 3 && val.length <= 10, message: '密码长度大于3小于10' }
      ],
      argeeRules: [{ validate: (val) => !!val, message: '必须同意用户协议' }],
      validateForm: {
        username: '',
        password: '',
        isAgree: false
      },
      menuList: [],
      openSimple: false
    }
  },
  components: {},
  created() {},
  mounted() {},
  methods: {
    openSimpleDialog() {
      this.openSimple = true
    },
    closeSimpleDialog() {
      this.openSimple = false
    },
    submit() {
      this.$refs.form.validate().then((result) => {
        console.log('form valid: ', result)
        //模拟后端接口数据
        let user = {
          userId: '2000100193',
          username: 'taoranran',
          userRole: 'admin',
          avatar: 'https://avatars1.githubusercontent.com/u/42235689?s=60&u=b25100f60b66465b78fe97e36b2788715c216a6d&v=4'
        }
        this.menuList = [
          { title: 'Dashboard', icon: 'mdi-view-dashboard', url: '/dashboard', subMenus: [] },
          {
            title: '音乐管理',
            icon: 'mdi-music',
            url: '',
            subMenus: [
              {
                title: '歌单管理',
                icon: 'mdi-domain',
                url: '/music-list',
                permissions: []
              },
              {
                title: '歌曲管理',
                icon: 'mdi-text',
                url: '/music',
                permissions: ['music:add', 'music:edit', 'music:delete']
              }
            ]
          },
          { title: 'About', icon: 'mdi-help-box', url: '/about', subMenus: [] }
        ]
        localStorage.setItem('token', 'EcIHTAWoGrmMVvTu2LPvuL-siq6hAfieVeehl-HTe_M')
        localStorage.setItem('user', JSON.stringify(user))
        localStorage.setItem('menuList', JSON.stringify(this.menuList))
        this.$store.commit('setToken', 'EcIHTAWoGrmMVvTu2LPvuL-siq6hAfieVeehl-HTe_M')
        this.$store.commit('setUser', user)
        this.$store.commit('setMenuList', this.menuList)
        this.$router.push('/')
        this.openSimpleDialog()
      })
    },
    clear() {
      this.$refs.form.clear()
      this.validateForm = {
        username: '',
        password: '',
        isAgree: false
      }
    }
  },
  computed: {}
}
</script>

<style scoped lang="scss">
.bg {
  background: -webkit-linear-gradient(to bottom, #7bc6cc, #be93c5);
  background: linear-gradient(to bottom, #7bc6cc, #be93c5); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */
  background-image: url('../assets/images/login-bg.jpg');
  opacity: 0.8;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.mu-demo-form {
  max-width: 500px;
  min-width: 500px;
  height: 350px;
  //   margin-left: 300px;
  background-color: #fff;
  border-radius: 10px;
  padding: 10px;
}
</style>
