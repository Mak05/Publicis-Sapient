<template>
  <div>
    <h1>{{heading}}</h1>
	Filter By :
	<select class="form-control" @change="launchFilter($event)">
		<option value="" selected disabled>Launching</option>
		<option v-for="launch in filters.launch" :value="launch" :key="launch">{{ launch }}</option>
	</select>
	<select class="form-control" @change="launchFilter($event)">
		<option value="" selected disabled>Landing</option>
		<option v-for="landing in filters.landing" :value="landing" :key="landing">{{ landing }}</option>
	</select>
	<select class="form-control" @change="launchFilter($event)">
		<option value="" selected disabled>Years</option>
		<option v-for="year in filters.years" :value="year" :key="year">{{ year }}</option>
	</select>
	<button @click="clear">Clear Filter</button>
    <ul v-if="data && data.length">
      <li v-for="launch in data" :key="launch.id">
        <img :src="launch.links.mission_patch_small"/>
        <div><strong>Flight No:</strong><p><strong>{{launch.flight_number}}</strong></p></div>
        <div><strong>Mission Name:</strong><p><strong>{{launch.mission_name}}</strong></p></div>
        <div><strong>Mission Details:</strong><p class="details">{{launch.details?launch.details:"none"}}</p></div>
        <div><strong>Launch Year:</strong><p>{{launch.launch_year}}</p></div>
      </li>
    </ul>

    <ul v-else-if="errors && errors.length">
      <li v-for="error in errors" :key="error.message">
        {{error.message}}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data () {
    return {
	  filters : {
		years: [2006, 2007, 2008, 2009, 2010, 2011, 2012, 2013, 2014, 2015, 2016, 2017, 2018, 2019, 2020],
		launch: ["true", "false"],
		landing: ["true", "false"],
	  },
      data: [],
	  launching: false,
	  landing: false,
	  year: "",
      heading: 'All Launches',
      errors: []
    }
  },

  // Fetches posts when the component is created.
  created () {
    this.fetch();
  },
  methods: {
	fetch () {
		axios.get('https://api.spacexdata.com/v3/launches?limit=100')
		  .then(response => {
			this.data = response.data
		  })
		  .catch(e => {
			this.errors.push(e)
		  })
	},
    launchFilter (event) {
		var url = "";
		var selectedVal = event.target.options[event.target.options.selectedIndex].text;
		var filter = event.target.options[0].text;
		if(filter === "Launching"){
			this.launching = selectedVal;
		}
		else if(filter === "Landing"){
			this.landing = selectedVal;
		}
		else if(filter === "Years"){
			this.year = event.target.options[event.target.options.selectedIndex].text;
		}
		url = "https://api.spacexdata.com/v3/launches?limit=100&launch_success="+this.launching+"&land_success="+this.landing+"&launch_year="+this.year;
		
		axios.get(url)
		  .then(response => {
			this.data = response.data;			
		  })
		  .catch(e => {
			this.errors.push(e)
		  })
    },
	clear (){
		var dropDowns = document.getElementsByClassName("form-control");
		for(var i = 0; i<dropDowns.length; i++){
			dropDowns[i].selectedIndex = 0;
		}
		this.fetch();
	}
	
  }
}
</script>

<style>
  ul {
      list-style-type: none;
  }
  li {
    width: 50%;
    float: left;
  }
  .home_active{
  display: none;
  }
  a {
    font-size: 30px;
    display: block;
    padding: 20px;
  }
  img {
    width: 90%;
  }
  p {
    display: inline-block;
  }
  strong {
    padding: 10px;
  }
  ul div {
    text-align: left;
    margin-left: 4%;
  }
  ul button {
    font-size: 24px;
    float: left;
    margin: 0 5% 5%;
    color: #ffffff;
    background-color: #07c5f8;
    padding: 10px 15px;
    border-radius: 40px;
  }
  p.details {
    width: 200px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    vertical-align: middle;
 }
</style>
