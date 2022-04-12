<template>
    <form @submit.prevent="searchNews" class="d-flex flex-column justify-content-center">
        <div class="input-group mx-sm-3 mb-2">
            <label class="visually-hidden" for="search">Search</label>
            <input type="search" name="search" v-model="searchTerm" id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
            <button class="btn btn-primary mb-2">Search</button>
        </div>
        <p>You are searching for {{ searchTerm }}</p>
    </form>


    <ul class="news__list">
        <li v-bind:v-for="article in articles" class="news__item">{{ article.title }}</li>
 </ul>
</template>


<script>
export default {
    data() {
        return {
            articles: [],
            searchTerm: ''
        };
    },

    methods: {
        searchNews() {
            let self = this;

            fetch('https://newsapi.org/v2/everything?q='+self.searchTerm + '&language=en', {
                headers: {
                    'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,
                        }
                    })
                    
                    .then(function(response) {
                        return response.json();
                        })
                        .then(function(data) {
                            console.log(data);
                            self.articles = data.articles;
                        });
                    }
    },   

    created() {
        let self = this;

        fetch('https://newsapi.org/v2/top-headlines?country=us',
{
    headers: {
        'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`
    }
})
        .then(function(response) {
            return response.json();
        })
        .then(function(data) {
            console.log(data);
            self.articles = data.articles;
        });
    }
};
</script>


<style>
div#articles{
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-row-gap: 2em;
    width: 100%;
    margin-top: 2em;
    justify-content: space-evenly;
    justify-items: center;
    align-content: space-evenly;
    align-items: center;
}
div.card img{
    object-fit: cover;
    height: 10rem;
}
div.card{
    width: 20rem !important;
    height: 35rem;
    border-bottom: solid #41B883 .3em;
}

</style>