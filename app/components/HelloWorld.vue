<template>
    <Page>
        <ActionBar title="Home" />
        <TabView height="100%" androidTabsPosition="bottom">


            <tabViewItem title="Tareas">
                <StackLayout orientation="vertical" width="100%"
                    height="100%">
                    <GridLayout columns="2*, *" rows="*" width="100%"
                        height="15%">

                        <TextField col="0" row="0" v-model="textFieldValue"
                            hint="Escribir tarea..." editable="true" />
                        <Button col="1" row="0" text="Agregar Tarea"
                            @tap="agregarTarea" />

                    </GridLayout>
            
                    <ListView for="tarea in tareas" @itemTap="onItemTap"
                        style="height:75%">
                        <v-template>
                            <label id="active-tarea" :text="tarea.nombre"
                                class="list-group-item-heading"
                                textWrap="true" />
                        </v-template>
                    </ListView>

                </StackLayout>
            </tabViewItem>



            <tabViewItem title="Completadas">

                <ListView id="completed-list" for="completo in completadas"
                    @itemTap="onDoneTap" height="100%">
                    <v-template>
                        <label id="completed-tarea" :text="completo.nombre"
                            class="list-group-item-heading" textWrap="true" />
                    </v-template>
                </ListView>
            </tabViewItem>
        </TabView>
    </Page>
</template>

<script>
    export default {
        methods: {
            onItemTap: function(args) {
                action("¿quedesea hacer con la tarea?", "cancelar", [
                    "completada",
                    "eliminar"
                ]).then(result => {
                    console.log(result);
                    switch (result) {
                        case "completada":
                            this.completadas.unshift(args.item);
                            this.tareas.splice(args.index, 1);
                            break;
                        case "eliminar":
                            this.tareas.splice(args.index, 1);
                            break;
                        case "cancelar" || undefined:
                            break;
                    }
                });
            },

            onDoneTap: function(args) {
			 action("¿quedesea hacer con la tarea?", "cancelar", [ "regresar", "eliminar" ]).then(result => {
                 console.log(result);
			        switch (result) {
                        case "regresar":
                            this.tareas.unshift(args.item);
                            this.completadas.splice(args.index, 1);
                            break;
                        case "eliminar":
                            this.completadas.splice(args.index, 1);
                            break;
                        case "cancelar" || undefined:
                            break;
                    }
                });
            },

            agregarTarea() {
                if (this.textFieldValue === "") return;
                this.tareas.unshift({
                    nombre: this.textFieldValue
                });
                this.textFieldValue = "";
            }
        },

        data() {
            return {
                tareas: [],
                completadas: [],
                textFieldValue: ""
            };
        }
    };
</script>

<style scoped>
    .home-panel {
        vertical-align: center;
        font-size: 20;
        margin: 15;
    }

    .description-label {
        margin-bottom: 15;
    }

    textField {
        font-size:20;
        color: #53ba82;
        margin-top: 10;
        margin-bottom:10;
        margin-right:5;
        margin-left:20;
    }

    Button {
        font-size: 15;
        font-weight: bold;
        color: black;
        backgound-color:#53ba82;
        height:40;
        margin-top:10;
        margin-bottom:10;
        margin-right:10;
        margin-left:10;
        border-radius:20px;
    }

    #active-tarea {
        font-size: 20;
        font-weight: bold;
        color: #53ba82;
        margin-left: 20;
        padding-top: 5;
        padding-bottom: 10;
    }

    #completed-tarea{
        font-size: 20;
        color: #d3d3d3;
        margin-left: 20;
        padding-top: 5;
        padding-bottom: 10;
    }

    #completed-tarea{
        font-size: 20;
        color: #3d3d3d;
        margin-left: 20;
        padding-top: 5;
        padding-bottom: 10;
        text-decoration: line-through;
    }
</style>