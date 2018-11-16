// Template
<template>
	<div class="container mx-auto px-4">
		<div v-for="story in stories" :key="story.id" class="rounded bg-white shadow mb-6 mx-auto px-4 py-6 w-full xl:w-3/5 relative story-item">
			<a :href="story.url" target="_blank" class="no-underline title-link">
				<h1 class="text-xl text-grey-darkest mb-1 story-item__title">{{ story.title }}</h1>
				
				<span v-show="story.url" class="text-grey no-underline font-normal story-item__link">
					— <span>{{ story.hostname }}</span>
				</span>
			</a>

			<span class="text-white story-item__votes text-sm flex font-medium items-center justify-center rounded-full absolute">{{ story.vote_count }}</span>
			
			<router-link :to="{ path: '/story/' + story.id }"  v-show="story.comment_count" class="button px-3 py-2 rounded text-sm font-medium no-underline absolute">View Post</router-link>
		</div>
	</div>
</template>


// Script
<script>
	import axios from 'axios'

	export default {
		name: 'Homepage',
		data: function() {
			return {
				err: '',
				stories: []
			}
		},
		created: function() {
			//https://www.designernews.co/api/v2/stories
			
			axios.get('https://www.designernews.co/api/v2/stories/')
			.then((result) => {
			
			let results = result.data.stories.slice(0, 10)
			
			results.forEach(element => {
					this.stories.push(element)					
				})				
			})
			.catch((err) => {
				this.err = err
			})
		}
	}	
</script>


// Styles
<style scoped>

	/* Story Item 
	------------- */
	.story-item {
		transition: box-shadow .3s cubic-bezier(0.25, 0.46, 0.45, 0.94); 
	}
	.story-item:hover {
		box-shadow: 0 4px 8px 0 rgba(0,0,0,0.12),
								0 2px 4px 0 rgba(0,0,0,0.08);
	}

	/* Story Item Title
	------------------- */
	.story-item__title { 
		font-weight: 600; 
	  max-width: 80%;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
		display: block;	
	}

	/* Story Item Link 
	 ----------------- */
	.story-item__link span {
		transition: all .3s cubic-bezier(0.445, 0.05, 0.55, 0.95);
		display: inline-block;
	}
	.title-link:hover .story-item__link span {
		transform: translateX(3px);
		color: #3E7BFF;
	}


	/* View Post Button 
	------------------- */
	.button {
		background: #3E7BFF;
		background-image: linear-gradient(120deg, #3E7BFF 0%, #66a6ff 100%);
		box-shadow: 0 2px 4px 0 rgba(62,123,255,0.2);
		color: #fff;
		top: 50%;	right: 1rem;
		transform: translateY(-50%);
		transition: all .3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
	}	
	.button:hover {
		box-shadow: 0 7px 14px rgba(62,123,255,0.3);
	}


	/* Votes 
	-------- */
	.story-item__votes {
		top: -12px; left: -10px;
		width: 30px; height: 30px;
		background: #5f6c87;
		box-shadow: 0 4px 10px 0 rgba(0, 0, 0, 0.19);
	}
	.story-item__votes:after {
		content: "votes";
		position: absolute;
		right: -38px;
		color: #5f6c87;
		opacity: 0;
		visibility: hidden;
		transition: opacity .3s cubic-bezier(0.445, 0.05, 0.55, 0.95);
	}

	.story-item__votes:hover:after {
		opacity: 1;
		visibility: visible;
	}
	

	/* Responsive  
	------------- */
	@media (max-width: 992px) {
		.button {	top: 90%; }
		.story-item__title { max-width: 90%; }	
	}	
</style>
