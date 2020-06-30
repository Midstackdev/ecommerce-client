<template>
	<form action="#" @submit.prevent="store">
		<div class="field">
		    <label for="card-element">
		      Credit or debit card
		    </label>
		    <div id="card-element">
		      <!-- A Stripe Element will be inserted here. -->
		    </div>

		    <!-- Used to display form errors. -->
		    <div id="card-errors" role="alert"></div>
		  </div>

		  <div class="filed">
		  	<p class="control">
		  		<button class="button is-info" :disabled="storing">
		  			Store card
		  		</button>
		  		<a href="#" class="button is-text" @click.prevent="$emit('cancel')">
		  			Cancel
		  		</a>
		  	</p>
		  </div>

	</form>
</template>

<script>
	

	export default {
		components: {
			
		},
		data () {
			return {
				stripe: null,
				card: null,
				storing: false,
				style: {
				  base: {
				    color: '#32325d',
				    fontFamily: '"Helvetica Neue", Helvetica, sans-serif',
				    fontSmoothing: 'antialiased',
				    fontSize: '18px',
				    '::placeholder': {
				      color: '#aab7c4'
				    }
				  },
				  invalid: {
				    color: '#fa755a',
				    iconColor: '#fa755a'
				  }
				},
				form: {
					
				}
			}
		},

		methods: {
			async store () {
				this.storing = true

				const { token, error } = await this.stripe.createToken(this.card)

				if (error) {

				}else {
					let response = await this.$axios.$post(`payment-methods`, {
						token: token.id
					})

					this.$emit('added', response.data)
				}

				this.storing = false
			}
		},

		mounted () {
			const stripe = Stripe('pk_test_O1rtQkDW73J6UG39dTf35MWv00BJoHrEP9')

			this.stripe = stripe

			this.card = this.stripe.elements().create('card')

			this.card.mount('#card-element', { style: this.style })
		}
	}
</script>


<style scope>
	.StripeElement {
	  box-sizing: border-box;

	  height: 40px;

	  padding: 10px 12px;

	  border: 1px solid transparent;
	  border-radius: 4px;
	  background-color: white;

	  box-shadow: 0 1px 3px 0 #e6ebf1;
	  -webkit-transition: box-shadow 150ms ease;
	  transition: box-shadow 150ms ease;
	}

	.StripeElement--focus {
	  box-shadow: 0 1px 3px 0 #cfd7df;
	}

	.StripeElement--invalid {
	  border-color: #fa755a;
	}

	.StripeElement--webkit-autofill {
	  background-color: #fefde5 !important;
	}
</style>