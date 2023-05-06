<template>
    <div class="w-96 mx-auto">
        <div>
            <input v-model="title" class=" w-96 mb-3 rounded-3xl border p-2 border-slate-400" type="text"
                   placeholder="title">
        </div>
        <div>
            <textarea v-model="content" class=" w-96 mb-3 rounded-3xl border p-2 border-slate-400"
                      placeholder="content"></textarea>
        </div>
        <div class="flex mb-3 items-center">
            <div>
                <input @change="storeImage" ref="file" type="file" class="hidden">
                <a href="#" class="block p-2 w-16 text-center text-sm rounded-3xl bg-sky-500 text-white"
                   @click.prevent="selectFile()">Image</a>
            </div>
            <div>
                <a v-if ="image" @click.prevent="image = null" class="ml-3" href="#">Cansel</a>
            </div>
        </div>
        <div v-if ="image">
            <img :src="image.url" alt="preview">
        </div>
        <div>
            <a @click.prevent="store" href="#" class="block p-2 w-32 text-center rounded-3xl bg-green-600 text-white
            hover:bg-white hover:border hover:border-green-600 hover:text-green-600 box-border ml-auto">Publish</a>
        </div>
    </div>

</template>

<script>

export default {
    name: "Personal",
    data() {
        return {
            title: '',
            content: '',
            image:null
        }
    },
    methods: {
        store(){
            const id = this.image_id ? this.image.id : null
            axios.post('/api/posts',{title: this.title, content:this.content, image_id: id})
                .then(res=>{
                    console.log(res)
                });
        },
        selectFile() {
            this.fileInput = this.$refs.file;
            this.fileInput.click();
        },
        storeImage(e) {
            const file = e.target.files[0]
            let formData = new FormData()
            formData.append('image', file)
            axios.post('/api/post_images', formData)
                .then(res => {
                    this.image = res.data.data
                })
        },
    }


}
</script>

<style scoped>

</style>
