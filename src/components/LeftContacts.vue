<template>
    <div>
        <b-modal id="bv-modal-example" v-model="show">
            <template v-slot:modal-title>
                <h3>Left Contacts</h3>
            </template>
            <b-form @submit="onSubmit" v-if="show">
                <b-form-group
                    id="input-group-1"
                    label="Email address:"
                    label-for="input-1"
                    description="We'll never share your email with anyone else."
                >
                    <b-form-input id="input-1" v-model="form.email" type="email" placeholder="Enter email"></b-form-input>
                </b-form-group>
                <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
                    <b-form-input id="input-2" v-model="form.name" placeholder="Enter name"></b-form-input>
                </b-form-group>
            </b-form>
            <template v-slot:modal-footer>
                <b-button variant="dark" size="lx" class="float-right" @click="show = false">
                    Close
                </b-button>
                <b-button variant="info" size="lx" class="float-right" type="submit" @click="orderIsDone">
                    Order
                </b-button>
            </template>
        </b-modal>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: {
                email: null,
                name: null
            },
            show: true
        };
    },
    watch: {
        show() {
            if (!this.show) {
                this.$emit("pp-close");
            }
        }
    },
    methods: {
        orderIsDone() {
            try {
                if (!this.form.email) {
                    throw new Error("Email is required!");
                }
                if (!this.form.name) {
                    throw new Error("Name is required!");
                }
            } catch (err) {
                this.$notify({
                    group: "foo",
                    title: "Error",
                    text: err.message
                });
                return;
            }
            this.show = false;
            this.$emit("orderIsDone", this.form);
        },
        onSubmit(evt) {
            evt.preventDefault();
            alert(JSON.stringify(this.form));
        }
    }
};
</script>

<style></style>
