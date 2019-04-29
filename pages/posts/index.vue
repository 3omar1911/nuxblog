<template>
  <v-layout>
    <v-flex xs12 sm6 offset-sm3>
      <v-card v-for="post in posts" class="my-5" :key="3254325">
        <v-card-title primary-title>
          <div>
            <h3 class="headline mb-0">{{ post.title }}</h3>
            <div> {{ post.description }} </div>
          </div>
        </v-card-title>

        <v-card-actions>
          <v-btn dark color="blue" router :to="'/posts/edit/' + post.id">Edit</v-btn>
          <v-btn dark color="red" @click.prevent="deletePost(post.id)">Delete</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>

	const axios = require('axios');
	const serverDomain = "http://localhost:8000";
	
	export default {
		layout: 'blog',
		
		data() {
			return {
				posts: [],
			}
		},

		mounted() {
			this.loadPosts();
		},
		methods: {
			loadPosts()
			{
				let url = '/api/posts';

				if(typeof this.$route.params.tag != 'undefined') {
					url += "?tag=" + this.$route.params.tag;
				}

				axios.get(serverDomain + url)
				.then( response => {
					this.posts = response.data;
				})
				.catch(err => {
				});
			},

			deletePost(postId)
			{
				axios.delete(serverDomain + '/api/posts/' + postId)
				.then(response => {
					this.loadPosts();
				})
				.catch(err => {
					console.log(err);
				});
			}
		}
	}
</script>