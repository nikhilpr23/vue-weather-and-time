<template>
    <div v-if="selected" class="weather">       
        <p class="title"> Location </p> 
        <p class="content" v-show="!loading">{{location}}<p/>
        <div class="centerwrap">
            <p class="spinner" v-show="loading"/>   
        </div> 
        <p class="title"> Temperature </p>         
        <p class="content temp" v-show="!loading">
            <span class="digital">{{temp}}{{String.fromCharCode(176)}}</span> C
        </p>
        <div class="centerwrap">
            <p class="spinner" v-show="loading"/>   
        </div> 
        <div class="descwrap" v-show="!loading">
            <img class="iconLocal" :src="icon"/>
            <p class="content">{{description}}</p>
        </div>
        <p class="title"> Time </p> 
        <p class="content" v-show="!loadingtime">{{time}}<p/>
        <div class="centerwrap">
            <p class="spinner" v-show="loadingtime"/>   
        </div> 
    </div>
    <div v-else class="empty">
        <span>Please pick a location to see details</span>
    </div>
    
</template>

<script>

export default {
    name: 'CurrentWeather',
    props: ['temp', 'location', 'time', 'description', 'icon', 'selected', 'loading', 'loadingtime']
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Iceland');

.weather {
    width: 100%;
}

.empty {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    color: white;
    font-size: 30px;
    font-weight: bold;
}

.empty span {
    margin: 10px 0;
    width: 40%;
}

.title {
    font-size: calc(var(--main-font-size)/2 + 5px);
    margin: calc(var(--main-font-size) - 5px) 0 0;
    text-align: center;
    text-shadow: 1px 1px 3px;
    color: rgb(48, 47, 47);  
}

.content {
    font-size: var(--main-font-size);
    color: white;
    font-weight: bold;
    margin: 0%;  
    padding: 0;
    text-shadow: 1px 1px 3px;
    text-align: center;
}

.temp {    
    font-size: calc(var(--main-font-size) + 10px); 
} 

.digital {
    font-size: calc(2 * var(--main-font-size) + 10px);  
    font-family: 'Iceland', cursive;
}

.centerwrap {
    margin: 0;
    padding: 0;
    text-align: center;
}

.descwrap{
    margin: 0;
    padding: 0;
    display: flex;
    align-items: center;
    flex-direction: column;
}

.iconLocal{
    width: calc(2 * var(--main-font-size));
    height: calc(2 * var(--main-font-size));
}

.spinner {
  border: calc(var(--main-font-size)/4 + 6px) solid #f3f3f3;
  border-radius: 50%;
  border-top: calc(var(--main-font-size)/4 + 6px) solid #252a2e;
  width: var(--main-font-size);
  height: var(--main-font-size);
  -webkit-animation: spin 2s linear infinite; /* Safari */
  animation: spin 2s linear infinite;
  display: inline-block;
}

/* Safari */
@-webkit-keyframes spin {
  0% { -webkit-transform: rotate(0deg); }
  100% { -webkit-transform: rotate(360deg); }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

p.content {text-transform:capitalize;}
</style>