<template>
    <v-container>
        <v-row>
            <v-col>
                <div class="headline">{{dataView.title}}</div>
            </v-col>
            <v-col cols="2">
                <v-btn block type="submit" form="saveProfile" color="primary" v-text="dataView.targetButton">
                </v-btn>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-form @submit.prevent="saveProfile" id="saveProfile" autocomplete="off">
                    <v-row>
                        <v-col md="6">
                            <v-text-field v-model="profile.name" id="inputName" name="name" label="Nombre del perfil">
                            </v-text-field>
                        </v-col>
                        <v-col md="6">
                            <v-text-field v-model="profile.description" id="inputDescription" name="name"
                                          label="Descripción del perfil">
                            </v-text-field>
                        </v-col>
                    </v-row>
                </v-form>
            </v-col>
        </v-row>
        <v-row>
            <v-col>
                <v-card>
                    <v-card-title>Permisos de este perfil</v-card-title>
                    <v-card-text>
                        <v-text-field v-model="searchSelfPermission" placeholder="Buscar. . ."
                                      id="inputSearchSelfPermission" name="profileName">
                        </v-text-field>
                    </v-card-text>
                    <v-sheet id="scrolling-techniques-2" class="overflow-y-auto" max-height="500">
                        <v-list style="height:400px;">
                            <v-list-item-group>
                                <draggable :list="selfPermissions" group="permissions">
                                    <v-list-item v-for="permission in filteredSelfPermissions"
                                                 v-text="permission._id" :key="permission._id">
                                    </v-list-item>
                                </draggable>
                            </v-list-item-group>
                        </v-list>
                    </v-sheet>
                </v-card>
            </v-col>
            <v-col>
                <v-card>
                    <v-card-title>Todos los permisos</v-card-title>
                    <v-card-text>
                        <v-text-field v-model="searchPermission" placeholder="Buscar. . ."
                                      id="inputSearchPermission" name="profileName2">
                        </v-text-field>
                    </v-card-text>
                    <v-sheet id="scrolling-techniques-3" class="overflow-y-auto" max-height="500">
                        <v-list style="height:400px;">
                            <v-list-item-group>
                                <draggable :list="allPermissions" group="permissions">
                                    <v-list-item v-for="permission in filteredPermissions"
                                                 v-text="permission._id" :key="permission._id">
                                    </v-list-item>
                                </draggable>
                            </v-list-item-group>
                        </v-list>
                    </v-sheet>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    import draggable from 'vuedraggable';
    export default {
        name: "SaveProfile",
        components: {
            draggable
        },
        data() {
            return {
                profile: {
                    _id: null,
                    name: null,
                    description: null,
                    permissions: []
                },
                dataView: {
                    title: '',
                    targetButton: ''
                },
                searchSelfPermission: '',
                searchPermission: '',
                selfPermissions: [],
                allPermissions: [
                    {_id: 'users-view'},
                    {_id: 'users-create'},
                    {_id: 'users-edit'},
                    {_id: 'users-delete'},
                ],
            }
        },
        created() {
            if (this.$router.currentRoute.name.includes("create")) {
                this.dataView.title = "Crear perfil";
                this.dataView.targetButton = "Crear";
            } else if (this.$router.currentRoute.name.includes("edit")) {
                this.dataView.title = "Editar perfil";
                this.dataView.targetButton = "Actualizar";
            }
        },
        methods: {
            saveProfile() {
                console.log("Perfil: ", this.profile);
            }
        },
        computed:{
            filteredSelfPermissions(){
                return this.selfPermissions.filter(permission=>{
                    return permission._id.toLowerCase().includes(this.searchSelfPermission.toLowerCase());
                })
            },
            filteredPermissions(){
                return this.allPermissions.filter(permission=>{
                    return permission._id.toLowerCase().includes(this.searchPermission.toLowerCase());
                })
            },
        }
    }
</script>

<style scoped>

</style>