<template>
<div>
   	<tags-search :tags="tags" @tagsChanged="updateTagIds"></tags-search>
  	<v-layout>
	    <v-flex xs12 sm6 offset-sm3>
	      <v-card v-for="post in currentPosts" class="my-5" :key="post.title">
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
	
</div>
</template>

<script>
	
	import TagsSearch from '../../components/posts/tags_search.vue';
	
	const axios = require('axios');
	const serverDomain = "http://localhost:8000";
	
	export default {
		layout: 'blog',
		components: {
			'tags-search': TagsSearch
		},
		data() {
			return {
				posts: [],
				tags: [],
				tagIds: [],
			}
		},

		mounted() {
			this.loadPosts();
			this.loadTags();
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
					// console.log(this.posts[0]);
				})
				.catch(err => {
				});
			},

			loadTags()
			{
				axios.get(serverDomain + '/api/tags')
				.then( response => {
					this.tags = response.data;
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
			},

			updateTagIds(ids)
			{
				this.tagIds = [];
				for(let index in ids) {
					this.tagIds.push(ids[index]);
				}
			},

			filterByTagIds()
			{
				let resultPosts = [];
				for(let i in this.posts) {
					let post = this.posts[i];
					let postTags = post.tags;
					if(this.postTagsIncluded(postTags)) {
						resultPosts.push(post);
					}
				}

				return resultPosts;
			},

			postTagsIncluded(postTags)
			{
				for(let index in postTags) {
					let tag = postTags[index];
					if(this.tagIds.includes(tag.id)) {
						return true;
					}
				}
				return false;
			}
		},
		computed: {
			currentPosts()
			{
				if(this.tagIds.length == 0) {
					return this.posts;
				}

				return this.filterByTagIds();
			}
		}
	}
</script>