<template>
	<article class="message">
	  <div class="message-body">
	    <h1 class="title is-5">Ship to</h1>
	    <template v-if="selecting">
	    	<ShippingAddressSelector 
				:addresses="addresses"
				:selectedAddress="selectedAddress"
				@clicked="addressSelected"
	    	/>
	    </template>

	    <template v-else-if="creating">
	    	<ShippingAddressCreator 
				@cancel="creating = false"
				@created="created"
	    	/>
	    </template>

	    <template v-else>
		    <template v-if="selectedAddress">
		    	<p>
		    		{{ selectedAddress.name }} <br>
			    	{{ selectedAddress.address_1 }} <br>
			    	{{ selectedAddress.city }} <br>
			    	{{ selectedAddress.postal_code }} <br>
			    	{{ selectedAddress.country.name }} <br>
			    </p>
			    <br>
		    </template>
		    <div class="field is-grouped">
		    	<p class="control">
		    		<a href="" class="button is-info" @click.prevent="selecting = true">Change shipping address</a>
		    	</p>
		    	<p class="control">
		    		<a href="" class="button is-info" @click.prevent="creating = true">Add shipping address</a>
		    	</p>
		    </div>
	    </template>
	  </div>
	</article>
</template>

<script>
	import ShippingAddressSelector from './ShippingAddressSelector'
	import ShippingAddressCreator from './ShippingAddressCreator'

	export default {
		components: {
			ShippingAddressSelector,
			ShippingAddressCreator
		},

		data () {
			return {
				localAddresses: this.addresses,
				selectedAddress: null,
				selecting: false,
				creating: false
			}
		},

		props: {
			addresses: {
				required: true,
				type: Array
			}
		},

		watch: {
			selectedAddress (address) {
				this.$emit('input', address.id)
			}
		},

		computed: {
			defaultAddress () {
				return this.localAddresses.find(a => a.default === true)
			}
		},

		methods: {
			addressSelected (address) {
				this.switchAddress(address)
				this.selecting = false
			},

			switchAddress (address) {
				this.selectedAddress = address
			},

			created (address) {
				this.localAddresses.push(address)

				this.creating = false

				this.switchAddress(address)
			}
		},

		created () {
			if (this.addresses.length) {
				this.switchAddress(this.defaultAddress)
			}
		}
	}
</script>