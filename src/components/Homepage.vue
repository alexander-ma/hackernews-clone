<template>
    <div>
        <item v-for="story in stories" :key="story.data.id" :story="story"/>
    </div>
</template>

<script>
import axios from 'axios'
import Item from '@/components/Item'
export default {
  name: 'Homepage',
  components: {
    'item': Item
  },
  data: function () {
    return {
      err: '',
      stories: []
    }
  },
  created: function () {
    axios.get('https://hacker-news.firebaseio.com/v0/topstories.json')
      .then((result) => {
        let results = result.data.slice(0, 15)
        results.forEach(id => {
          axios.get('https://hacker-news.firebaseio.com/v0/item/' + id + '.json')
            .then((result) => {
              this.stories.push(result)
            })
            .catch((err) => {
              this.err = err
            })
        })
      })
      .catch((err) => {
        this.err = err
      })
  }
}
</script>

<style scoped>

</style>
