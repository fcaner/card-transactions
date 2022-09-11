<template>
    <div>
        <label>Amount & Description Filter</label>
        <input type="text" v-model="search" placeholder="Amount or Description" />
        <div class="transactions" :class="clickedIndex===0 ?'debitTransaction' : 'creditTransaction'"
            v-for="item in  search ? filteredTransactions() : selectedCardTransactions" v-bind:key="item.id">
            <div className="description"> {{ item.description }}</div>
            <div className="amount"> {{ item.amount }}</div>
        </div>
        <div v-if="search&&!filteredTransactions().length">
            <p>No results found!</p>
        </div>
        <div v-if="!selectedCardTransactions">
            <p>There is no any transaction yet!</p>
        </div>
    </div>
</template>
<script>


export default {
    name: 'TransactionList',
    props: {
        cardId: String,
        clickedIndex: Number
    },
    data() {
        return {
            selectedCardTransactions: [],
            search: ""
        };
    },
    methods: {
        getData(newVal) {
            fetch('./data/transactions.json')
                .then(async response => {
                    const data = await response.json();
                    if (!response.ok) {
                        const error = (data && data.message) || response.statusText;
                        return Promise.reject(error);
                    }
                    this.selectedCardTransactions = data.transactions[newVal];
                    return data;
                })
                .catch(error => {
                    this.errorMessage = error;
                });
        },
        filteredTransactions() {
            return this.selectedCardTransactions.filter(transaction => {
                return this.search > 0 ? this.search <= transaction.amount : transaction.description.toLowerCase().includes(this.search.toLowerCase())
            });
        }
    },
    watch: {
        cardId: function (newVal) {
            this.getData(newVal);
            this.search = "";
        }
    }
}

</script>

<style>
input[type=text] {
    width: 100%;
    padding: 12px 20px;
    margin: 10px 0;
    display: inline-block;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
}

.transactions {
    width: 100%;
    padding: 20px 20px;
    margin: 8px 0;
    display: inline-block;
    border-radius: 4px;
    box-sizing: border-box;
    display: flex;
    justify-content: space-between;
    color: azure;
}

.debitTransaction {
    background: linear-gradient(to right, #48D7D8, #007DBB);
}

.creditTransaction {
    background: linear-gradient(to right, #0287C6, #043665);
}

label {
    margin-top: 30px;
    display: flex;
    font-size: 12px;
    color: rgba(0, 0, 0, .50);
    transition: .15s all ease-in-out;
}
</style>