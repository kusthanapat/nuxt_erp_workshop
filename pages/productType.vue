<script setup>
    import config from '@/config';
    import axios from 'axios';
    import Swal from 'sweetalert2';

    const showModal = ref(false);

    definePageMeta({
        layout: "admin"
    })

    const name = ref('');
    const remark = ref('');

    const closeModal = () => {
        showModal.value = false;
    }

    //save
    const save = async () => {
        try {
            const payload = {
                name: name.value,
                remark: remark.value
            }

            const res = await axios.post(config.apiServer + '/api/productType/create', payload);
        } catch (e) {
            Swal.fire({
                icon: 'error',
                title: 'Error',
                text: e.message,
            })
        }
    }
</script>

<template>
    <div>Product Type</div>
    <div class="mt-3">
        <button class="btn btn-primary" @click="showModal = true">
            <i class="fa fa-plus"></i>
            Add Product Type
        </button>
    </div>

    <Modal v-if="showModal" title="Add Product Type" @close="closeModal">
        <div>Name</div>
        <input type="text" class="form-control" v-model="name" />

        <div class="mt-3">Remark</div>
        <input type="text" class="form-control" v-model="remark" />

        <button class="btn btn-primary mt-3" @click="save">
            <i class="fa fa-check me-2"></i>
            Save
        </button>
    </Modal>
</template>