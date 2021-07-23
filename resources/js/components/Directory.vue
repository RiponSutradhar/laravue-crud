import axios from '../../../public/js/app';
<template>
    <div>
        <div class="form-group">
        <label for="name">Name</label>
        <input type="text" id="name" class="form-control" placeholder="Enter Your Name" v-model="item.name">
        </div>
        <div class="form-group">
        <label for="phone">Phone</label>
        <input type="text" id="phone" class="form-control" placeholder="Enter Phone Number" v-model="item.phone">
        </div>
        <button class="btn btn-success btn-block" @click="save">{{ isEditing? 'Update':'Save' }}</button>
        <div class="col-md-12 mt-5" v-if="lists.length>0">
        <h2 class="text-center"> Telephone Numbers </h2>
        <ul class="list-group" v-for="item in lists" :key="item.id">
            <li class="list-group-item ">{{ item.name }} => {{ item.phone }}
            <span class="float-right">

            <button class="btn btn-sm btn-warning mr-2" @click="editItem(item)">Edit</button>
            <button class="btn btn-sm btn-danger mr-2" @click="deleteItem(item.id)">Delete</button>
            </span>

            </li>
        </ul>
        </div>
    </div>

</template>

<script>
export default {
    name:'Directory',
    data (){
        return{
            lists: [],
            item:{
                name: "" ,
                phone: ""
            },
            isEditing: false,
            temp_Id:null
        }
    },
    mounted() {
        this.fetchAll();
    },
    methods:{
        fetchAll(){
                axios.get('api/tel')
                .then(res => this.lists=res.data);
            },
        save (){
            try{
                let method=axios.post;
                let url='api/tel';
                if(this.isEditing==true){
                    method=axios.put;
                    url=`api/tel/${this.temp_Id}`;
                }

                method(url, this.item)
                .then(res=>{
                    this.item = {
                        name: "",
                        phone: ""
                    };
                    this.fetchAll();
                    this.isEditing=null;
                    this.temp_Id=false;
                });

            }
            catch (e) {
                console.log(e);
            }
        },
        deleteItem(id){
           try{
                axios.delete(`api/tel/${id}`)
            .then(res=>this.fetchAll());
           }
           catch(e){
               console.log('Error is occured', e);
           }
        },
        editItem (data){
            this.item = {
                name:data.name,
                phone:data.phone
            },
            this.temp_Id=data.id;
            this.isEditing=true;
        }
    }
};
</script>

<style></style>
