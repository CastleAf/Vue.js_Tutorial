<template>
	<div class="user-profile">
		<div class="user-profile__sidebar">
			<div class="user-profile__user-panel">
				<h1 class="user-profile__username"> @{{ state.user.username }} </h1>
				<div class="admin-badge" v-if="state.user.isAdmin === true">
					Admin
				</div>
				<div class="user-profile__followerCount">
					<strong>Followers:</strong> {{ state.followers }}
				</div>
			</div>
			<CreateTwootPanel @add-twoot="addTwoot"/>
		</div>
		<div class="user-profile__twoots-wrapper">
			<TwootItem 
				v-for="twoot in state.user.twoots" 
				:key="twoot.id" 
				:username="state.user.username" 
				:twoot="twoot" 
				@favourite="toggleFavourite" 
			/>
		</div>
	</div>
</template>

<script>
import { users } from "./../assets/users"
import TwootItem from "./../components/TwootItem"
import CreateTwootPanel from "./../components/CreateTwootPanel"
import { reactive, computed } from 'vue'
import { useRoute } from 'vue-router'

export default {
	name: 'App',
	components: { TwootItem, CreateTwootPanel },
	setup() {

		const route = useRoute();
		const userId = computed(() => route.params.userId);
		
		const state = reactive({

			newTwootContent: '',
			selectedTwootType: '',
			twootTypes: [
				{ value: 'draft', name: 'Draft'},
				{ value: 'instant', name: 'Instant Twoot'}
			],
			followers: 0,
			user: users[userId.value - 1] || users[0]
		})

		const fullname = computed(() => `${state.user.firstName} ${state.user.lastName}`)
		const newTwootCharacterCount = computed(() => state.newTwootContent.length)

		function followUser() {
			state.followers++;
		}

		function addTwoot(twoot) {
			state.user.twoots.unshift({ id: state.user.twoots.length + 1, content: twoot })
		}

		return {
			state,
			fullname,
			newTwootCharacterCount,
			followUser,
			addTwoot,
			userId
		}
	},

	watch: {

		followers(newFollowers, oldFollowers) {
			if (oldFollowers < newFollowers) {
				console.log(`${this.user.username} has gained a new Follower!!`)
			}
			else {
				console.log(`${this.user.username} has lost a follower :(`)
			}
		}
	},

	mounted () {
		this.followUser()
		console.log("done")
	}
};
</script>

<style lang="scss" scoped>

.user-profile {
	display: grid;
	grid-template-columns: 1fr 3fr;
	width: 100%;
	padding: 50px 5%;


	.user-profile__user-panel {
		display: flex;
		flex-direction: column;
		padding: 20px;
		background-color: white;
		border-radius: 5px;
		border: 1px solid #DFE3E8;
		margin-bottom: auto;
	}

	h1 {
		margin: 0;
		padding: 5px 0;
	}
	
	.admin-badge {
		background: teal;
		color: white;
		border-radius: 5px;
		margin-right: auto;
		padding: 0px 10px;
		font-weight: bold;
	}
}

.user-profile__twoots-wrapper {
	margin-left: 60px;
	display: grid;
	padding-right: 10%;
	grid-gap: 10px;
	margin-bottom: auto;
}

</style>