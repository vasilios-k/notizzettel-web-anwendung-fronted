<template>
  <h1>Notes</h1>
  <div class="col" v-for="notes in notes" :key="notes.id">
    <div class="p-3 mb-2 bg-light text-dark" style="max-width: 18rem;">
    <div :class="getColor (notes)">{{notes.category}}</div>
    <div class="card-body">
      <h5 class="card-title">{{ notes.headline }}</h5>
      <p class="card-text">{{notes.text}}</p>
      <div class="card-footer bg-transparent border-success">{{ notes.person.firstName }} {{ notes.person.lastName }}</div>
      <div class="card-footer text-muted"> {{getDate (notes)}}
      </div>
    </div>
  </div>
  </div>
</template>

<script>

export default {
  name: 'NotesView',
  data () {
    return {
      notes: []
    }
  },
  methods: {
    getColor (notes) {
      if (notes.category === 'JOB') {
        return 'card text-white bg-primary mb-3'
      } else {
        return 'card text-white bg-success mb-3'
      }
    },
    getDate (notes) {
      const datesave = new Date(notes.date)
      const datenow = new Date()
      const diffTime = Math.abs(datenow - datesave)
      const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24))
      let diffYearTime = (datenow.getTime() - datesave.getTime()) / 1000
      diffYearTime /= (60 * 60 * 24)
      const diffYears = Math.abs(Math.round(diffYearTime / 365.25))
      if (diffDays <= 1) { return datesave.getHours() + ':' + datesave.getMinutes() } // heute erstellt zeigt heutige uhrzeit an
      if (diffDays === 2) { return 'Gestern' + ' ' + datesave.getHours() + ':' + datesave.getMinutes() } // gestern erstellt zeigt gestrige uhrzeit an
      if (diffYears > 1) {
        return datesave.getDay() + '.' + datesave.getMonth() + '.' + datesave.getFullYear() // wenn die notiz über 1 jahr alt ist wird das jahr mit angezeigt
      } else return datesave.getDay() + '.' + datesave.getMonth() // zeigt tag und monat der notiz wenn unter 1 jahr
    }
  },

  mounted () {
    const endpoint = process.env.VUE_APP_BACKEND_BASE_URL + '/api/notes'
    const requestOptions = {
      method: 'GET',
      redirect: 'follow'
    }
    fetch(endpoint, requestOptions)
      .then(response => response.json())
      .then(result => result.forEach(notes => {
        this.notes.push(notes)
      }))
      .catch(error => console.log('error', error))
  }
}
</script>

<style scoped>
</style>
