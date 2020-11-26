<template>
   <div>
       
        <table class="table">
            <tr v-for="item in students"  v-bind:key="item._id"> 
                <td>{{ item.name }}</td><td><input type="checkbox" v-model="item.isDonePr"></td><td>{{item.group}}</td><td>{{ item.mark}}</td>
                <td><a href = "#" @click="deleteSt(item._id)">Удалить</a></td>
                <td v-if = "item.name != search"><a href = "#" @click="changeVal(item.name)">Изменить</a></td>
                <td v-if = "item.name == search">
                    <a href = "#" @click="changeSt(item._id)">Изменино</a>
                </td>
                
            </tr>
            
        </table>
        
        
        <div class = "search">
            <input  type="text" v-model = "student.name">
            <select v-model="student.group">
                 <option value="RPZ 17 1/9">РПЗ 17 1/9</option>
                <option value="RPZ 17 2/9">РПЗ 17 2/9</option>
            </select>
            <input type="number" v-model = "student.mark">
            <input type = "checkbox" v-model = "student.isDonePr"> ПР
            <button v-on:click="addSt()">Добавить</button>
        </div> 
         </div>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
 
    export default {
       data: function() {
           return {
                search:"",
                students: [],
                student: {name:"", group:"", mark:"", isDonePr:""}
           };
        },
        mounted: function(){      
            this.updata();
        },
        methods: {
            deleteSt: function(id){
                Vue.axios.delete("http://46.101.212.195:3000/students/" + id)
                .then((response) => {
                    this.updata();
                })
            },
            changeVal: function(name){
                this.student = this.students.find(id => {
                    if(id.name == name){
                        return id
                    }
                });
                this.search = name
            },
           changeSt: function(id){
                Vue.axios.put("http://46.101.212.195:3000/students/" + id,{
                    name: this.student.name,
                    group: this.student.group,
                    mark: this.student.mark,
                    isDonePr: this.student.isDonePr
                })
                .then((response) => {
                    this.updata();
                })
            },

            addSt: function(){
                Vue.axios.post("http://46.101.212.195:3000/students",{
                    name: this.student.name,
                    group: this.student.group,
                    mark: this.student.mark,
                    isDonePr: this.student.isDonePr
                })
                
                .then((response) => {
                    this.updata();
                })
            },
            updata: function(){
                Vue.axios.get("http://46.101.212.195:3000/students").then((response) => {
              
                this.students = response.data;
                })
            }
        }
    }
</script>