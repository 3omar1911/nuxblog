<template>
  <v-toolbar
    dark
    color="teal"
  >
    <v-toolbar-title>Tags</v-toolbar-title>
    <v-autocomplete
      v-model="select"
      :loading="loading"
      :items="items"
      :search-input.sync="search"
      cache-items
      class="mx-3"
      flat
      hide-no-data
      hide-details
      label="Search Tags?"
      solo-inverted
      :multiple="true"
      @change="emitTagChange"
    ></v-autocomplete>
  </v-toolbar>
</template>

<script>
  export default {
    props: ['tags'],
    data () {
      return {
        loading: false,
        items: [],
        search: null,
        select: null,
      }
    },
    watch: {
      search (val) {
        val && val !== this.select && this.querySelections(val)
      }
    },
    methods: {
      querySelections (v) {
        this.loading = true
        this.items = this.computedTags.filter(e => {
          return (e.text || '').toLowerCase().indexOf((v || '').toLowerCase()) > -1
        })
        this.loading = false
      },

      emitTagChange()
      {
        this.$emit('tagsChanged', this.select);
      }
    },
    computed: {
      computedTags()
      {
        let objs = [];
        for(let index in this.tags)
        {
          objs.push({text: this.tags[index].title, value: this.tags[index].id});
        }
        return objs;
      },
    }
  }
</script>
