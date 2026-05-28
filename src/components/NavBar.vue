<script setup lang="ts">
const props = defineProps<{ currentPage: number; totalPages: number }>()
</script>

<template>
    <nav class="navbar">
        <div class="page-heading">
            <div class="page-title">
                <router-link to="/">
                    <img src="/Makeup%20Productslogo.png" alt="Makeup Products logo" class="page-logo" />
                </router-link>
            </div>
            <p>Browse all products, 10 items per page, with simple page navigation.</p>
            <div class="summary">Page {{ props.currentPage }} of {{ props.totalPages }}</div>
        </div>
    </nav>
</template>


<style scoped>
.page-logo {
    max-width: 260px;
    height: auto;
}

.page-heading h1 {
    margin: 0;
    font-size: clamp(2rem, 2.4vw, 2.4rem);
    color: var(--primary-4);
}

.page-heading p {
    margin: 10px 0 0;
    color: var(--text-muted);
    max-width: 560px;
}

.page-title {
    display: flex;
    align-items: center;
    gap: 18px;
}

.page-heading {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 18px;
    flex-wrap: wrap;
    margin-bottom: 26px;
    padding: 24px 24px 18px;
    background: rgba(230, 73, 140, 0.08);
    border: 1px solid rgba(230, 73, 140, 0.14);
    border-radius: 24px;
}
</style>