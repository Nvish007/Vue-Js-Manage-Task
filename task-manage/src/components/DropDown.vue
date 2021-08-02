<template>
	<div>
  <select class="form-control" @change="changeJobTitle($event)">
    <option value="" selected disabled>Choose</option>
    <!-- <option v-for="jobTitle in jobTitles" :value="jobTitle.id" :key="jobTitle.id">{{ jobTitle.name }}</option> -->
  </select>
  <br><br>
  <!-- <p><span>Selected job title: {{ selectedJobTitle  }}</span></p> -->
</div>
</template>

<script>
export default {
	name: 'DropDown',
}
</script>