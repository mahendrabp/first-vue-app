<template>
    <div id="employee-form">
        <form @submit.prevent="handleSubmit">
            <label>Employee name</label>
            <input ref="first" type="text" :class="{ 'has-error': submitting && invalidName }"
                v-model="employee.name" />
            <label>Employee Address</label>
            <input type="text" :class="{ 'has-error': submitting && invalidAddress }"
                v-model="employee.address.street" />
            <p v-if="error && submitting" class="error-message">
                ❗Please fill out all required fields
            </p>
            <p v-if="success" class="success-message">
                ✅ Employee successfully added
            </p>

            <button>Add Employee</button>
        </form>
    </div>
</template>

<script>
    export default {
        name: "employee-form",
        data() {
            return {
                submitting: false,
                error: false,
                success: false,
                employee: {
                    name: "",
                    address: {
                        street: ''
                    },
                }
            };
        },
        methods: {
            handleSubmit() {
                this.submitting = true


                if (this.invalidName || this.invalidAddress) {
                    this.error = true
                    return
                }
                this.$emit('add:employee', this.employee)
                this.$refs.first.focus()
                this.employee = {
                    name: '',
                    address: {
                        street: ''
                    },
                }
                this.error = false
                this.success = true
                this.submitting = false
            }
        },

        computed: {
            invalidName() {
                return this.employee.name === ''
            },

            invalidAddress() {
                return this.employee.address.street === ''
            },
        },
        // clearStatus() {
        //     this.success = false
        //     this.error = false
        // },
    };
</script>

<style scoped>
    form {
        margin-bottom: 2rem;
    }

    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: #d33c40;
    }

    .success-message {
        color: #32a95d;
    }
</style>