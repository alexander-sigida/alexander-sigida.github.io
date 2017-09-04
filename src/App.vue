<template>
 	<div class="container">
		<app-header :quoteCount="quotes.length" :maxQuotes="maxQuotes"></app-header>
		<app-new-quote @quoteAdded="addNewQuote"></app-new-quote>
 		<app-quote-grid :quotes="quotes" @quoteDeleted="deleteQuoteInArray"></app-quote-grid>
 		<div class="row">
 			<div class="col-xs-12 text-center">
 				<div class="alert alert-info">Info: click a Quote to delete it!</div>
 			</div>
 		</div>
 	</div>
</template>

<script>
	import Header from './components/Header.vue';
	import QuoteGrid from './components/QuoteGrid.vue';
	import NewQuote from './components/NewQuote.vue';

	export default {
		data: function() {
			return {
				quotes: [
					'Just a Quote to see something',
				],
				maxQuotes: 10
			}
		},
		components: {
			appQuoteGrid: QuoteGrid,
			appNewQuote: NewQuote,
			appHeader: Header
		},
		methods: {
			addNewQuote(quote) {
				if (this.quotes.length >= this.maxQuotes) {
					alert("You've reached maximum number of quotes!");
				} else {
					this.quotes.push(quote);
				}
			},
			deleteQuoteInArray(index) {
				this.quotes.splice(index, 1);
			}
		},

	}
</script>

<style>

</style>
