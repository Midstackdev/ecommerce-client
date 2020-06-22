<template>
	<tr>
	  <td>
	    #{{ order.id }}
	  </td>
	  <td>
	    {{ order.created_at }}
	  </td>
	  <td>
	    <div v-for="variation in products" :key="variation.id">
	      <nuxt-link 
	      	:to="{
	      		name: 'products-slug',
	      		params: {
	      			slug: variation.product.slug
	      		}
	      	}"
	      >
	      	{{ variation.product.name }} ({{ variation.name }}) - {{ variation.type }}
	      </nuxt-link>
	    </div>
	    <template v-if="moreProducts > 0">
	      and {{ moreProducts }} more
	    </template>
	  </td>
	  <td>{{ order.subtotal }}</td>
	  <td>
	    <component :is="order.status" :order="order" />
	  </td>
	</tr>
</template>


<script>
	import OrderStatusPending from '@/components/orders/statuses/OrderStatus-pending'
	import OrderStatusPaymentFailed from '@/components/orders/statuses/OrderStatus-payment_failed'

	export default {
		components:{
			'pending': OrderStatusPending,
			'payment_failed': OrderStatusPaymentFailed,
		},

		props: {
			order: {
				required: true,
				type: Object
			}
		},

		data () {
			return {
				maxProducts: 2,
				stausClasses: {
		    		'is-success' : this.order.status === 'completed',
		    		'is-info' : this.order.status === 'processing' || this.order.status === 'pending',
		    		'is-danger' : this.order.status === 'payment_failed'
		    	}
			}
		},

		computed: {
			products () {
				return this.order.products.slice(0, this.maxProducts)
			},

			moreProducts () {
				return this.order.products.length - this.maxProducts
			}
		}
	}
</script>