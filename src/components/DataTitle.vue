<template>
    <div class="text-center mb-10">
        <h2 class="text-3xl font-bold mb-3">
            {{ title }}
        </h2>
        <div class="text-2xl">{{timestamp}}</div>
    </div>
    
</template>

<script>
import moment from 'moment'

export default {
    name: 'DataTitle',
    props: ['title', 'dataDate'],

    computed: {
        timestamp: function() {
            return moment(this.dataDate).format('MMMM Do YYYY h:mm:ss a')
        }
    }
}

</script>
