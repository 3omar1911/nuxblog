<template>
	<v-navigation-drawer
      v-model="drawer"
      fixed
      app
    >
      <v-list dense>
        <v-list-tile v-for="item in listItems" router :to="item.route" :key="item.key">
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title>{{ item.title }}</v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
</template>

<script>
	export default {
		props: ['drawer'],
		
		data() {
			return {
				listItems: [
					{title: "Dashbard", icon: "home", route: '/dashboard', key: 1},
					{title: "All Posts", icon: "home", route: '/posts/', key: 2},
					{title: "Create Post", icon: "home", route: '/posts/create', key: 3},
				]
			}
		}
	}
</script>