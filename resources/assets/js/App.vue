<template>
	<div class="container">
		<div class="navbar">
			<div class="navbar__brand">
				<a class="navbar__brand">
					<img id="brand-image" alt="Jela svijeta" src="http://plavatvornica.com/wp-content/themes/plavatvornica/images/logo.png" />
				</a>
				<router-link to="/">Jela svijeta</router-link>
			</div>
			<ul class="navbar__list">
				<li class="navbar__item" v-if="!check">
					<router-link to="/login">LOGIN</router-link>
				</li>
				<li class="navbar__item" v-if="!check">
					<router-link to="/register">REGISTER</router-link>
				</li>
				<li class="navbar__item" v-if="check">
					<router-link to="/recipes/create">CREATE RECIPE</router-link>
				</li>
				<li class="navbar__item" v-if="check">
					<a @click.stop="logout">LOGOUT</a>
				</li>
			</ul>
		</div>
    <div class="flash flash__success" v-if="flash.success">
			{{flash.success}}
		</div>
    <div class="flash flash__error" v-if="flash.error">
			{{flash.error}}
		</div>
		<router-view></router-view>
	</div>
</template>
<script type="text/javascript">
	import auth from './store/auth'
  import flash from './helpers/flash'
	import { post } from './helpers/api'
  export default {

		created() {

			auth.initialize()

		},

    data() {

      return {

        flash: flash.state,
				auth: auth.state


      }

    },
		computed: {

			check() {

				if(this.auth.api_token && this.auth.user_id) {
						return true
				}
						return false
			}

		},
		methods: {

				logout() {

						post('api/logout')
								.then((res) => {

										if(res.data.logged_out) {

												auth.remove()
												flash.setSuccess('You have successfully logged out!')
												this.$router.push('/login')

										}

								})

				}
		}


  }
</script>
