<template>
	<div>
		<manage-posts></manage-posts>
	</div>
</template>
<script>
	import ManagePosts from '../../components/posts/manage.vue';

	export default {
		layout: 'blog',
		components: {
			'manage-posts': ManagePosts,
		}
	}
</script>