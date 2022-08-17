<template>
    <form @submit.prevent="handleSubmit()" class="house-form">
        <h3 class="text-primary">List Your House</h3>
        <div class="row">

        <div class="col-4">
            <label class="form-label" for="name">Bedrooms</label>
            <input class="form-control" type="text" id="bedrooms" name="bedrooms" v-model="editable.bedrooms">
        </div>

        <div class="col-4">
            <label class="form-label" for="name">Bathrooms</label>
            <input class="form-control" type="text" id="bathrooms" name="bathrooms" v-model="editable.bathrooms">
        </div>

        <div class="col-4">
            <label class="form-label" for="name">Levels</label>
            <input class="form-control" type="text" id="levels" name="levels" v-model="editable.levels">
        </div>

        <div class="col-4">
            <label class="form-label" for="year">Year</label>
            <input class="form-control" type="number" id="year" name="year" v-model="editable.year">
        </div>

        <label class="form-label" for="price">Price</label>
        <input class="form-control" type="number" min="1" id="price" name="price" v-model="editable.price">

        <label class="form-label" for="img">Image</label>
        <input class="form-control" type="text" id="img" name="img" v-model="editable.img">

        <label class="form-label" for="description">Description</label>
        <textarea class="w-100 form-control" name="description" id="description" rows="5" v-model="editable.description"></textarea>

        <button type="submit" class="btn btn-primary w-100 p-2 mt-3 text-light">Submit</button>
        </div>
    </form>
</template>


<script>
// FIXME log ref?? 
import { watchEffect } from 'vue';
import { AppState } from '../AppState.js';
import { housesService } from '../services/HousesService.js';
import { logger } from '../utils/Logger.js';
import Pop from '../utils/Pop.js';

export default {
    setup(){
        const editable = ref({})

        watchEffect(() => {
            if(!AppState.activeHouse){return}
            editable.value = {...AppState.activeHouse}
        })

        return{
            editable,
            async handleSubmit(){
                try {
                    if(editable.value.id){
                        housesService.editHouse(editable.value)
                    } else{
                        housesService.createHouse(editable.value)
                    }
                    editable.value = {}
                } catch (error) {
                    logger.error('[Creating or Editing House]', error)
                    Pop.error(error)
                }
            }
        }
    }
}
</script>


<style lang="scss" scoped>
</style>