<template>
    <div>
        <input
            type="checkbox"
            @change="updateCheck()"
            v-model="item.completed"
        />
        <span :class="[item.completed ? 'completed' : '', 'itemText']"> {{ item.name }}</span>
        <button @click="removeItem()" class="trashCan">
            <font-awesome-icon icon="trash"/>
        </button>

    </div>
</template>

<script>
export default {
    props: ['item'],
    name: "listItem.vue",
    methods: {
        updateCheck() {
            axios.put('api/item/' + this.item.id, {
                item: this.item
            })
                .then(response => {
                    if (response.status === 200) {
                        this.$emit('itemchanged')
                    }
                })
                .catch(error => {
                        console.log(error);
                    }
                )
        },
        removeItem() {
            axios.delete('api/item/' + this.item.id)
                .then(response => {
                    if (response.status === 200) {
                        this.$emit('itemchanged')
                    }
                })
                .catch(error => {
                        console.log(error);
                    }
                )
        }
    }
}
</script>

<style scoped>
.completed {
    text-decoration: line-through;
    color: #999999;
}

.itemText {
    width: 100%;
    margin-left: 20px;
}

.item {
    display: flex;
    justify-content: center;
}

.trashCan {
    background: #e6e6e6;
    border: none;
    color: red;
    outline: none;
}

</style>
