<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8">
                <router-link
                :to="{ name: 'transaction.create' }"
                class="btn btn-primary shadow mb-3"
                >
                Add
                </router-link>

                <div class="card shadow">
                    <div class="card-header">Transaction List</div>
                        <div class="card-body">
                            <table class="table">
                                <thead>
                                    <tr>
                                    <th>Title</th>
                                    <th>Amount</th>
                                    <th>Type</th>
                                    <th>Action</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(transaction, index) in transactions.data" :key="index">
                                    <td>{{transaction.title}}</td>
                                    <td>{{transaction.amount}}</td>
                                    <td>{{transaction.type}}</td>
                                    <td>
                                        <div class="btn-group">
                                        <router-link
                                            :to="{ name: 'transaction.edit', params: { id: transaction.id } }"
                                            class="btn btn-sm btn-info text-white"
                                        >
                                            Edit
                                        </router-link>
                                        </div>
                                        <button class="btn btn-sm btn-danger" @click="destroy(transaction.id, index)">Delete</button>
                                    </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
</template>

<script>
import axios from 'axios'
import {onMounted, ref} from 'vue'

export default {
    setup(){
        // state
        let transactions = ref([]);

        onMounted(() => {
        // get data from endpoint
        axios.get('http://127.0.0.1:8000/api/transaction')
        .then((result) => {
            transactions.value = result.data
        }).catch((err)=>{
            console.log(err.response)
        });
        });

        function destroy(id, index) {
            axios.delete(`http://127.0.0.1:8000/api/transaction/${id}`)
        .then(() => {
            transactions.value.data.splice(index, 1)
        }).catch((err)=>{
            console.log(err.response.data)
        });
        }

        return {
        transactions,
        destroy
        }
    }
};
</script>
