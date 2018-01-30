<!-- This component is used with the tabs to filter tournaments -->
<template>
	<div class="row">
		<div v-show="isLoading">
			<div class="alert alert-success">Loading tournaments</div>
		</div>
		<div v-show="!isLoading" class="col-md-4" v-for="tournament in tournaments">
			<tournament :tournament="tournament" :key="tournament.id"></tournament>
		</div>
	</div>
</template>

<script>
	import Bus from '../bus.js';
	export default {

		// this is the collection of tournaments. Passed via server instead of an Ajax request
		props: ['data'],

		data() {
			return {
				tournaments: [],
				isLoading: true,
			}
		},

		methods: {
			filterTournaments(selector) {
				// check if the tournament's slug matches the tab's slug.
				// return only the tournaments that match the slug
				if (selector == 'all') { return this.tournaments = this.data }
				this.tournaments = this.data.filter(tournament => {
					return tournament.platform.slug == selector;
				});
			}
		},

		mounted() {
			Bus.$on('filter.selected', (data) => {
				this.filterTournaments(data.slug);
			});
		},

		created() {
			this.tournaments = this.data;
			this.isLoading = false;
		},
	}
</script>	