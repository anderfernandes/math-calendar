<template>
  <div class="container-fluid">
    <div class="row">
      <h2>
        <i class="glyphicon glyphicon-calendar"></i>
        On This Day in Math 
        <small>{{ format(new Date(), 'EEEE, MMMM d') }}</small>
      </h2>
      <div class="thumbnail" v-for="event in events" :key="event.uuid">
        <div class="caption">
          <h3>{{ formatTitle(event.title) }}</h3>
          <p v-html="event.description"></p>
        </div>
      </div>
    </div>
  </div>
  
</template>

<script>
  
  import format from 'date-fns/format'
  import axios from 'axios'

  // URL with feed converted into JSON
  const url = "https://api.rss2json.com/v1/api.json?rss_url=https://www.maa.org/news/on-this-day/rss.xml"

  export default {
    data: () => ({
      events: [],
    }),
    async created() {
      await this.fetchEvents()
    },
    methods: {
      format,
      async fetchEvents() {
        try {
          const response = await axios.get(url)
          Object.assign(this, { events: response.data.items })
        } catch (error) {
          alert(`Unable to fetch events: ${ error.message }`)
        }
      },
      formatTitle(date) {
        let parsedDate = date.split("-")
        let month = parsedDate[0] - 1
        let day   = parsedDate[1]
        let year  = parsedDate[2]
        let d = new Date(year, month, day)
        d = this.format(d, 'EEEE, MMMM d yyyy')
        return d
      }
    },
  }
  
</script>