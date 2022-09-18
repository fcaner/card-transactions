<template>
    <div className="cardsAndTransactions">
        <div :class="mobile ? 'mobileCard' : 'desktopCard'">
            <img class="image" :class="selectedIndex===0 ?'active' : 'notActive'" @click="onCardClick(0)" alt=""
                src="../assets/Debit.jpeg">
            <img class="image" :class="selectedIndex===1 ?'active' : 'notActive'" @click="onCardClick(1)" alt=""
                src="../assets/Kredit.jpeg">
        </div>
        <div v-if="showTransaction">
            <TransactionList :cardId="this.selectedCardId" :clickedIndex="selectedIndex" />
        </div>
    </div>
</template>
<script>

import TransactionList from './TransactionList.vue'

export default {
    name: 'CardList',
    components: {
        TransactionList
    },
    methods: {
        onCardClick(clickedIndex) {
            this.selectedIndex = clickedIndex;
            fetch('./data/cards.json')
                .then(async response => {
                    const data = await response.json();
                    if (!response.ok) {
                        const error = (data && data.message) || response.statusText;
                        return Promise.reject(error);
                    }
                    this.selectedCardId = clickedIndex === 0 ? data.cards[0].id : data.cards[1].id;

                }).catch(error => {
                    this.errorMessage = error;
                });
            this.showTransaction = true;
        }
    },
    data() {
        return {
            selectedCardId: "",
            description: "",
            showTransaction: false,
            selectedIndex: "",
            mobile: window.innerWidth <= 700
        };
    },
    created() {
        addEventListener('resize', () => {
            this.mobile = innerWidth <= 700
        })
    }
}
</script>

<style>
.image {
    max-width: 300px;
    max-height: 300px;
    width: 40%;
    height: auto;
    cursor: pointer;
    transition: box-shadow 0.5s ease-in-out;
    border-radius: 4%;
}

.image:hover,
.active {
    box-shadow: rgb(0 0 0 / 60%) 5px 5px 20px 5px;
    transition: box-shadow 0.5s ease-in-out;
}

.desktopCard {
    display: flex;
    justify-content: space-evenly;
}

.mobileCard {
    display: flex;
    justify-content: space-between;
}

.cardsAndTransactions {
    margin: 40px 40px;
}
</style>