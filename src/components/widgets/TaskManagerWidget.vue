<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">Task_Operations</h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="add_task()" title="Add task">+</WidgetHeaderButton>
            <WidgetHeaderButton @click="show_modal=true">O</WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <template v-if="task_array.length < 1">

                <p>Add a task</p>

            </template>
        
            <template v-if="task_array.length > 0">
                <template
                v-for="(item,index) in task_array" 
                :key="item.id">
                    <div 
                    v-if="item.checked == true"  
                    class="form-check d-flex ps-4 mb-3 fade-effect fade-task">
                        <input class="form-check-input checkbox-size" type="checkbox" value="" checked>
                        <label class="form-check-label pt-1 px-3 task-size" for="flexCheckDefault">
                            <span>
                                {{item.msg}}
                            </span>
                        </label>
                    </div>
                    <div 
                    v-if="item.checked == false"  
                    class="form-check d-flex ps-4 mb-3 fade-effect">
                        <input @change="remove_task(index)" class="form-check-input checkbox-size" type="checkbox" value="">
                        <label class="form-check-label pt-1 px-3 task-size text-success" for="flexCheckDefault">
                            <span contenteditable="true" @input="update_task_item(index,$event)">
                                {{item.msg}}
                            </span>
                        </label>
                    </div>
                </template>
            </template>

        </div>
    </div>

</template>

<script>

import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";

export default {
    name: "TaskManagerWidget",
    props: ['prop_name'],
    data() {
        return {
            task_array: [],
        }
    },
    methods: {
        add_task() {

            var _new = {
                'checked': false,
                'msg': 'Sample task'
            }

            this.task_array.unshift(_new);

        },
        check_uncheck(index) {
            this.task_array[index]['checked'] != this.task_array[index]['checked'];
        },
        update_task_item(index,e){

            //
            //
            if (e.target.innerText !== this.task_array[index]['msg']) {
                this.$emit("update:task_array[index]['msg']", e.target.innerText)
            }

        },
        remove_task(index){

            //
            //
            this.task_array[index]['checked'] = true
            console.log(index)

            //
            // wait for 1,5 seconds before removing
            setTimeout(() => {
                this.task_array.splice(index,1);
            }, 2000);

        }
    },
    components: {
        WidgetHeaderButton
    },
};
</script>

<style scoped>

    .fade-effect {
        opacity: 1;
    }

    .fade-task {
        opacity: 0.5;
    }
    .checkbox-size {
        font-size: 0.8rem;
    }
    .task-size {
        font-size: 0.7rem;
        font-family: "Space Grotesk", monospace;
    }

</style>