<template>
    <v-container>
        <v-row justify="center">
            <v-col xs="12" sm="12" md="10" lg="8" xl="5">
                    <div class="d-flex flex-row-reverse mb-5">
                        <v-tooltip bottom>
                            <template v-slot:activator="{on}">
                                <v-btn color="success" v-on="on" fab dark :to="{name:'home.users.create'}">
                                    <v-icon>add</v-icon>
                                </v-btn>
                            </template>
                            <span>Agregar usuario</span>
                        </v-tooltip>
                    </div>
                    <v-data-table :headers="headers" :items="users" sort-by="name" class="elevation-1">
                        <template v-slot:item.action="{item}">
                            <v-tooltip bottom>
                                <template v-slot:activator="{on}">
                                    <v-icon color="info" v-on="on" small class="mr-2" @click="openEditUser(item)">
                                        edit
                                    </v-icon>
                                </template>
                                <span>Editar</span>
                            </v-tooltip>
                            <v-tooltip bottom>
                                <template v-slot:activator="{on}">
                                    <v-icon color="error" v-on="on" small class="mr-2" @click="openRemoveModal(item)">
                                        delete
                                    </v-icon>
                                </template>
                                <span>Eliminar</span>
                            </v-tooltip>
                        </template>
                        <template v-slot:body.append="{isMobile}">
                            <tr v-if="!isMobile">
                                <td>
                                    <v-text-field v-model="headersFilter.name" type="text"
                                                  label="Nombre"></v-text-field>
                                </td>
                                <td>
                                    <v-text-field v-model="headersFilter.username" type="text"
                                                  label="Usuario"></v-text-field>
                                </td>
                                <td>
                                    <v-text-field v-model="headersFilter.email" type="email"
                                                  label="Correo"></v-text-field>
                                </td>
                            </tr>
                        </template>
                    </v-data-table>
                    <modal-remove ref="refModalRemove" v-bind:modalData="userTemp"
                                  @id_element="deleteUser"></modal-remove>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    import ModalRemove from "../../components/Utilities/Modals/ModalRemove";

    export default {
        name: "ListUsers",
        components: {
            ModalRemove
        },
        data() {
            return {
                headersFilter: {
                    name: '',
                    username: '',
                    email: ''
                },
                users: [
                    {_id: 1, name: 'Peter Parker', username: 'Spider-Man', email: 'spidy@correo.com'},
                    {_id: 2, name: 'Tony Stark', username: 'Iron Man', email: 'iron@correo.com'},
                    {_id: 3, name: 'Steve Rogers', username: 'Captain America', email: 'captain@correo.com'}
                ],
                userTemp: {
                    preposition: 'al',
                    typeElement: 'usuario',
                    mainNameElement: '',
                    _id: null,
                    element: {}
                }
            }
        },
        computed: {
            headers() {
                return [
                    {
                        value: 'name', text: 'Nombre', sortable: true, filter: value => {
                            return value != null && typeof value === 'string' &&
                                value.toString().toLocaleLowerCase()
                                    .indexOf(this.headersFilter.name.toLocaleLowerCase()) !== -1;
                        }
                    },
                    {
                        value: 'username', text: 'Usuario', sortable: true, filter: value => {
                            return value != null && typeof value === 'string' &&
                                value.toString().toLocaleLowerCase()
                                    .indexOf(this.headersFilter.username.toLocaleLowerCase()) !== -1;
                        }
                    },
                    {
                        value: 'email', text: 'Correo', sortable: true, filter: value => {
                            return value != null && typeof value === 'string' &&
                                value.toString().toLocaleLowerCase()
                                    .indexOf(this.headersFilter.email.toLocaleLowerCase()) !== -1;
                        }
                    },
                    {value: 'action', text: 'Opciones', sortable: false}
                ]
            }
        },
        methods: {
            openEditUser(user) {
                //TODO: Guardar en Vuex
                console.log("user: ", user);
                this.$router.push({name: 'home.users.edit'});
            },
            openRemoveModal(user) {
                console.log("user: ", user);
                this.userTemp.element = user;
                this.userTemp._id = user._id;
                this.userTemp.mainNameElement = user.name;
                this.userTemp.element = user;
                this.$refs.refModalRemove.dialog = true;
            },
            deleteUser(idUser) {
                console.log("Usuario a eliminar: ", idUser);
            }
        }
    }
</script>

<style scoped>

</style>