<template>
    <div>
        <notifications group="foo" />
        <Shop />
    </div>
</template>

<script>
import Shop from "@/components/Shop";

export default {
    name: "App",
    components: {
        Shop
    }
};
</script>

<style></style>
