<template>
	<div class="user-profile">
		<div class="user-profile__sidebar">
			<div class="user-profile__user-panel">
				<h1 class="user-profile__username"> @{{ user.username }} </h1>
				<div class="admin-badge" v-if="user.isAdmin === true">
					Admin
				</div>
				<div class="user-profile__followerCount">
					<strong>Followers:</strong> {{ followers }}
				</div>
			</div>
			<CreateTwootPanel @add-twoot="addTwoot"/>
		</div>
		<div class="user-profile__twoots-wrapper">
			<TwootItem 
				v-for="twoot in user.twoots" 
				:key="twoot.id" 
				:username="user.username" 
				:twoot="twoot" 
				@favourite="toggleFavourite" 
			/>
		</div>
	</div>
</template>

<script>
import TwootItem from "./TwootItem"
import CreateTwootPanel from "./CreateTwootPanel"

export default {
	name: 'App',
	components: { TwootItem, CreateTwootPanel },
	data() {

		return {
			newTwootContent: '',
			selectedTwootType: '',
			twootTypes: [
				{ value: 'draft', name: 'Draft'},
				{ value: 'instant', name: 'Instant Twoot'}
			],
			followers: 0,
			user: {
				id: 1,
				username: '_CastleAf',
				firstName: 'Afonso',
				lastName: 'Castelão',
				email: 'sampleEmail@hotmail.com',
				isAdmin: true,
				twoots: [
					{ id: 4, content: 'Twotter is Amazing <3!!', isFav: false},
					{ id: 3, content: 'Omggg Jessica Did this to mooi ://', isFav: false},
					{ id: 2, content: 'Buying gf, 3€', isFav: false},
					{ id: 1, content: 'green orange. that\'s it, that\'s the twoot', isFav: false}
				]
			}
		}
	},
	computed: {

		fullName() {
			return `${this.user.firstName} ${this.user.lastName}`;
		},

		newTwootCharacterCount() {
			return this.newTwootContent.length;
		},

		newTwootCharacterLeft() {
			if (this.newTwootContent.length < 100) {
				return 100 - this.newTwootContent.length;
			}
			else {
				return 0;
			}
		}
	},

	methods: {

		followUser() {
			this.followers++;
		},

		toggleFavourite(id) {

			for (var i = 0; i < this.user.twoots.length; i++) {
				var twoot = this.user.twoots[i]
				console.log(twoot)
				if (twoot.id == id) {
					console.log("Ok found it!!")
					this.user.twoots[i].isFav = true
				}
			}
		},

		addTwoot(twoot) {
			this.user.twoots.unshift({ id: this.user.twoots.length + 1, content: twoot })
		},

		createNewTwoot() {
			if (this.newTwootContent.length != 0 && this.selectedTwootType != '' && this.selectedTwootType !== 'draft') {
				
				// unshift puts it at the start of the list
				this.user.twoots.unshift({
					id: this.user.twoots.length + 1,
					content: this.newTwootContent,
					isFav: false
				})
				this.newTwootContent = ''
				this.selectedTwootType = ''
				console.log("Twoot submited!!")
			}
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