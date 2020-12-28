<template>
    <!-- :class - if project.complete is true, apply class 'complete' -->
    <div class="project" :class="{ complete: project.complete }">
        <div class="actions">
            <h3 @click="displayDetailsToggle">{{ project.title }}</h3>
            <div class="icons">
                <span class="material-icons">edit</span>
                <span  @click="deleteProject" class="material-icons">delete</span>
                <span  @click="toggleComplete" class="material-icons check">done</span>
            </div>
        </div>
        <div class="details" v-if="displayDetails">
            <p>{{ project.details }}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: ['project'],
    data() {
        return {
            displayDetails: false,
            uri: 'http://localhost:3000/projects/' + this.project.id
        }
    },
    methods: {
        displayDetailsToggle() {
            this.displayDetails = !this.displayDetails
        },
        // the emit is being listented to in Home.vue in  <SingleProject />
        deleteProject() {
            fetch(this.uri, { method: 'DELETE'})
            .then(() => this.$emit('delete', this.project.id))
            .catch(err => console.log(err.message))
        },
        // Patch - the same as UPDATE
        toggleComplete() {
            fetch(this.uri, {
                method: 'PATCH',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ complete: !this.project.complete})
            }).then(() => this.$emit('complete', this.project.id))
            .catch(err => console.log(err.message))
        }
    }
    
}
</script>

<style>
.project {
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 4px solid #e90074;
}
h3 {
    cursor: pointer;
}
.actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.material-icons {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
}
.material-icons:hover {
    color: #777;
}
.project.complete {
    border-left: 4px solid #00ce89;
}
.project.complete .check {
    color: #00ce89;
}
</style>
