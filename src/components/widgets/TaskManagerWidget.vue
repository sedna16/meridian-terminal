<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="add_task()" title="Add task">
                <PlusSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>
            <WidgetHeaderButton @click="update_panel(true)">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <template v-if="widget_data.task_array.length < 1">

                <a 
                @click.prevent="update_panel(true)" 
                href="#" 
                class="text-success text-decoration-none">
                    Add a task
                </a>

            </template>
        
            <template v-if="widget_data.task_array.length > 0">
                <template
                v-for="(item,index) in widget_data.task_array" 
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
                            <span 
                            contenteditable="true" 
                            v-once 
                            @input="update_task_item(index,$event)">
                                {{item.msg}}
                            </span>
                        </label>
                    </div>
                </template>
            </template>

        </div>
    </div>

<TaskManagerSP v-if="widget_data.show_panel==true" @update-panel="update_panel" :widget_data="widget_data" />

</template>

<script>

import PlusSVG from "@/components/svg/PlusSVG.vue";
import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import TaskManagerSP from "@/components/sidepanels/TaskManagerSP.vue";

export default {
    name: "TaskManagerWidget",
    props: ['widget_index','widget_data'],
    data() {
        return {
        }
    },
    methods: {
        move_widget(direction){

            //
            //
            this.$parent.move_widget(this.widget_index,direction);

        },
        delete_widget(){
            this.$parent.delete_widget(this.widget_index);
        },
        update_panel(v) {
            this.$parent.hide_all_panel();
            this.widget_data.show_panel = v;
        },

        //
        //
        update_session(){
            this.$parent.update_session();
        },

        //
        //
        add_task() {

            //
            //
            var _new = {
                'checked': false,
                'msg': 'Sample task'
            }

            //
            //
            this.widget_data.task_array.unshift(_new);

        },
        check_uncheck(index) {
            this.widget_data.task_array[index]['checked'] != this.widget_data.task_array[index]['checked'];
            this.update_session();
        },
        update_task_item(index,e){

            //
            //
            // if (e.target.innerText !== this.widget_data.task_array[index]['msg']) {
            //     this.$emit("update:widget_data.task_array[index]['msg']", e.target.innerText)
            // }

            //
            //
            this.widget_data.task_array[index].msg = e.target.innerText;
            this.update_session();

        },
        remove_task(index){

            //
            //
            this.widget_data.task_array[index]['checked'] = true

            //
            // wait for 1,5 seconds before removing
            setTimeout(() => {
                this.widget_data.task_array.splice(index,1);
            }, 500);

            //
            //
            this.update_session();

        }
    },
    components: {
        PlusSVG,
        GearSVG,
        WidgetHeaderButton,
        TaskManagerSP,
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