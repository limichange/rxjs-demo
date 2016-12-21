<template>
  <div id="app">
    <button ref="button" type="button" name="button">点击</button>
    <p v-text="count"></p>
  </div>
</template>

<script>
import Rx from 'rxjs'

export default {
  data () {
    return {
      count: 0
    }
  },
  mounted () {
    console.clear()
    const self = this

// =============================================================================

    const Observer = Rx.Observable.ajax('https://api.github.com')

    Observer
      .map(res => res.response.code_search_url)
      .subscribe(console.log)

    Observer
      .map(res => res.response.issue_search_url)
      .subscribe({
        next: console.log
      })

// =============================================================================

    Rx.Observable.fromEvent(self.$refs.button, 'click')
      .throttleTime(1000)
      .scan(count => count + 1, self.count)
      .subscribe({
        next: (count) => {
          self.count = count
        }
      })
  }
}
</script>

<style>
button {
  width: 200px;
  font-size: 28px;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
