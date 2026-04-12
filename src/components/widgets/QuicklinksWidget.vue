<template>
  <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

            <h6 class="m-0 pb-0 mb-0 d-inline-block">
                {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
            </h6>

            <div class="btn-group float-end">

                <WidgetHeaderButton @click.prevent="open_panel()">
                    <GearSVG w="12" h="12" c="var(--bs-light)" />
                </WidgetHeaderButton>

            </div>

        </div>
        <div class="card-body p-3">

            <a 
            v-if="widget_data.widget_data.qlinks_array.length < 1" 
            @click.prevent="update_panel(true)" 
            href="#" 
            class="text-success text-decoration-none">
                Add new links
            </a>

            <template 
            v-if="widget_data.widget_data.qlinks_array.length > 0">

                <template 
                v-for="(item,index) in widget_data.widget_data.qlinks_array" 
                :key="item.id">
                    <a 
                    class="d-block mb-3 text-success text-decoration-none truncate link-item"
                    :href="item.link" 
                    :title="item.name" 
                    target="_blank">{{item.name}}</a>
                </template>

            </template>
            

        </div>
  </div>

<QuicklinksSP 
v-if="show_panel==true" 
@hide-panel="hide_panel" 
:widget_data="widget_data" 
/>

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import QuicklinksSP from "@/components/sidepanels/QuicklinksSP.vue";

export default {
    name: "QuicklinksWidget",
    props: ['widget_index','widget_data','base_time','show_panel'],
    data() {
        return {

        }
    },
    created(){

    },
    methods: {

        //
        //
        move_widget(direction){

            //
            //
            this.$parent.move_widget(this.widget_index,direction);

        },
        update_widget(){
            this.$parent.update_widget(this.widget_data.id);
        },
        delete_widget(){
            this.$parent.delete_widget(this.widget_index);
        },
        open_panel(){
            this.$parent.open_panel(this.widget_data.id);
        },
        hide_panel(v) {
            this.$parent.hide_panel();
        },

        //
        //
        add_link(){
            this.widget_data.widget_data.qlinks_array.push({
                'name': 'Google',
                'link': 'https://www.google.com/',
            })
            this.$parent.update_widget(this.widget_data.id);
        },
        remove_link(index){
            this.widget_data.widget_data.qlinks_array.splice(index,1);
            this.$parent.update_widget(this.widget_data.id);
        },
        update_link(index,key,new_value){
            this.widget_data.widget_data.qlinks_array[index][key] = new_value;
            this.$parent.update_widget(this.widget_data.id);
        },

    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        QuicklinksSP,
    },
};
</script>

<style scoped>

    .link-item {
        background-color: rgba(var(--bs-success), 0);
        padding: 0.5rem;
        border: 1px solid transparent;
    }
    .link-item:hover {
        background-color: rgba(var(--bs-success), 0.7);
        padding: 0.5rem;
        border: 1px solid var(--bs-success)
    }
    .link-item::before {
        content: "> ";
    }
    .truncate {
        width: 100%; /* Or any width/max-width */
        white-space: nowrap; /* Prevents wrapping */
        overflow: hidden; /* Hides the extra text */
        text-overflow: ellipsis; /* Adds "..." */
    }

</style>
