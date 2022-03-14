<template>
  <div class="project" :class="{complete:project.complete}">
      <div class="flexing">
            <div>
                <h3 @click="showDetail=!showDetail">{{project.title}}</h3>
            </div>
            <div>
                <span class="material-icons" @click="deleteProject">
                    delete
                </span>
                <span class="material-icons">
                    edit
                </span>
                <span class="material-icons" @click="completeProject">
                    done
                </span>
            </div>
        </div>
        <p v-if="showDetail">{{project.detail}}</p>
      
  </div>
</template>

<script>
export default {
    props:['project'],
    data(){
        return{
            showDetail:false,
            api:"http://localhost:3000/projects/"
        }
    },
    methods:{
        deleteProject(){
            let deleteRouter=this.api+this.project.id;
            fetch(deleteRouter,{method:"DELETE"})
            .then(()=>{
                this.$emit("delete",this.project.id)
            })
            .catch((err)=>{
                console.log(err)
            })
        },
        completeProject(){
            let updateCompleteRout=this.api+this.project.id;
            fetch(updateCompleteRout,{
                method:"PATCH",
                headers:{
                    "Content-type":"application/json"
                },
                body:JSON.stringify(
                    {
                        complete:!this.project.complete
                    }
                )
            })
            .then(()=>{
                this.$emit("complete",this.project.id)
            })
            .catch((err)=>{
                console.log(err)
            })
        }
    }
}
</script>

<style>
.project{
    padding: 20px;
    background-color: #eeecec;
    border-radius: 10px;
    margin: 10px;
    border-left: 8px solid rgb(253, 45, 87);
}
h3{
    color: rgb(118, 5, 199);
    cursor: pointer;
}
.flexing{
    display: flex;
    justify-content: space-between;
    align-items: center;
    

}
span{
    margin-left: 10px;
}
span:hover{
    color: rgb(116, 117, 117);
    cursor: pointer;
}
.complete{
    border-left-color: rgb(2, 156, 2);
}

</style>