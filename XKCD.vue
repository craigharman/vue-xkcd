<template>
    <div class="xkcd text-center">
        <h2>{{ title }}</h2>
        
        <div v-if="loading" class="loader loader--style1" title="0">
            <svg version="1.1" id="loader-1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
            width="40px" height="40px" viewBox="0 0 40 40" enable-background="new 0 0 40 40" xml:space="preserve">
            <path opacity="0.2" fill="#000" d="M20.201,5.169c-8.254,0-14.946,6.692-14.946,14.946c0,8.255,6.692,14.946,14.946,14.946
                s14.946-6.691,14.946-14.946C35.146,11.861,28.455,5.169,20.201,5.169z M20.201,31.749c-6.425,0-11.634-5.208-11.634-11.634
                c0-6.425,5.209-11.634,11.634-11.634c6.425,0,11.633,5.209,11.633,11.634C31.834,26.541,26.626,31.749,20.201,31.749z"/>
            <path fill="#000" d="M26.013,10.047l1.654-2.866c-2.198-1.272-4.743-2.012-7.466-2.012h0v3.312h0
                C22.32,8.481,24.301,9.057,26.013,10.047z">
                <animateTransform attributeType="xml"
                attributeName="transform"
                type="rotate"
                from="0 20 20"
                to="360 20 20"
                dur="0.5s"
                repeatCount="indefinite"/>
                </path>
            </svg>
        </div>
        <div v-else>
            <img :title="title" id="xkcdimg" :src="image" class="img-responsive center-block form-margin-bottom" :alt="alt">
            <p><button id="anotherXKCDBtn" class="btn btn-primary" v-on:click="showXKCD()">Another</button></p>
        </div>
    </div>
</template>

<script>
import Vue from 'vue'
import VueResource from 'vue-resource'
Vue.use(VueResource);

export default {
    data: function() {
        return {
            image: '',
            alt: '',
            title: '',
            loading: true
        }
    },
    methods: {
        showXKCD: function () {
            this.image = ''
            this.alt = 'Loading...'
            this.title = 'Loading...'

            var newest_url = "http://dynamic.xkcd.com/api-0/jsonp/comic/";

            this.$http.jsonp(newest_url).then(response => {
                var xkcd = JSON.parse(response.bodyText)
                var max_num = xkcd.num
                var rand_num = Math.floor((Math.random() * max_num) + 1)
                //var url = "https://xkcd.com/" + rand_num + "/info.0.json"
                var url = "http://dynamic.xkcd.com/api-0/jsonp/comic/" + rand_num
                this.$http.jsonp(url).then(response => {
                    var randomXkcd = JSON.parse(response.bodyText)
                    this.loading = false
                    this.image = randomXkcd.img
                    this.alt = randomXkcd.alt
                    this.title = randomXkcd.title
                })
            })
        }
    }, mounted() {
        this.showXKCD()
    }

}
</script>

<style>
.loader{
  margin: 0 0 2em;
  height: 100px;
  width: 20%;
  text-align: center;
  padding: 1em;
  margin: 0 auto 1em;
  display: inline-block;
  vertical-align: top;
}

/*
  Set the color of the icon
*/
svg path,
svg rect{
  fill: #FF6700;
}
</style>