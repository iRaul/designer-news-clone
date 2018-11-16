// Template
<template>
  <div id="app">
		<navbar></navbar>
    <router-view/>
  </div>
</template>


// Script
<script>
import Navbar from '@/components/Navbar'

export default {
	name: 'App',
	components: {
		Navbar
	}
}
</script>


// Styles
<style>
	body { 
		font-family: 'Barlow', sans-serif; 
		background: #F3F6F9;	
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
	}
</style>
