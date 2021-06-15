<template>
  <div class="date">
    <p>{{today.day}}</p>
    <p>{{today.date}}</p>
  </div>
</template>

<script>
export default {
    name: 'Data',

    data () {
      const current = new Date()
      const weekday = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
      const date = `${current.getDate()}/${current.getMonth()+1}/${current.getFullYear()}`

      const today = {
        day:  weekday[current.getDay()],
        date:  date,
      }
      
      return {
          today
      }
    }
};
</script>