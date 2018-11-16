// Template
<template>
  <div>
    <div class="container mx-auto px-4">
      <div class="mx-auto md:w-4/5 shadow-md bg-white p-4 rounded">
        <div v-for="story in stories" :key="story.id" class="w-full mb-8">
          <h2 class="font-semibold mb-2">{{ story.title }}</h2>
            
          <p class="leading-normal">{{ story.comment }}</p>      
        </div>

        <h3 class="font-medium mb-3">Comments:</h3>
        <div v-for="comment in comments" :key="comment.id" class="w-full py-6 border-b border-grey-light">
          <div v-html="comment.body_html">{{ comment.body }}</div>
        </div>        
      </div>
    </div>
  </div>
</template>


// Script
<script>
	import axios from 'axios'

  export default {
    name: 'Single',
    data: function() {
      return {
        stories: [],
        comments: []
      }
    },
		created: function() {
			//https://www.designernews.co/api/v2/stories
			
			axios.get('https://www.designernews.co/api/v2/stories/' + this.$route.params.id)
			.then((result) => {	
        let results = result.data.stories;
        
        results.forEach(element => {
          this.stories.push(element)

          let comments = element.links.comments

          comments.forEach(id => {
            axios.get('https://www.designernews.co/api/v2/comments/' + id)
            .then((res) => {
              let comments = res.data.comments

              comments.forEach(comment => {
                this.comments.push(comment)
              })
            })
          })
        })
      })
      .catch((err) => {
        console.log(err)
      })
		}
  }
</script>


// Styles
<style scoped>

</style>
