<script setup lang="ts">
import { computed, ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import ProductCard from '../components/ProductCard.vue'
import NavBar from '@/components/NavBar.vue'
import type { Product } from '../types/product'
import products from '../../products.json'

const allProducts = products as Product[]
const pageSize = 10
const route = useRoute()
const router = useRouter()

const placeholderPath = '/images/placeholder.svg'
// track product ids that should be hidden from the listing (frontend-only)
const hiddenIds = ref(new Set<number | string>())

const availableProducts = computed(() => {
    return allProducts.filter(p => {
        if (!p) return false
        if (hiddenIds.value.has(p.id)) return false
        const img = (p.image_link || '').toString()
        if (!img) return false
        // exclude any product using the placeholder image
        if (img.includes('placeholder.svg')) return false
        return true
    })
})

const totalPages = computed(() => Math.max(1, Math.ceil(availableProducts.value.length / pageSize)))
const currentPage = computed(() => {
    const page = Number(route.query.page ?? 1)
    if (!Number.isFinite(page) || page < 1) return 1
    if (page > totalPages.value) return totalPages.value
    return Math.floor(page)
})

const paginatedProducts = computed(() => {
    const start = (currentPage.value - 1) * pageSize
    return availableProducts.value.slice(start, start + pageSize)
})

const goToPage = (page: number) => {
    if (page < 1 || page > totalPages.value) return
    router.push({ name: 'home', query: { page: String(page) } })
}

const hideProduct = (id: number | string) => {
    hiddenIds.value.add(id)
}
</script>

<template>
    <section class="home-page">
        <NavBar :current-page="currentPage" :total-pages="totalPages" />

        <div class="product-grid">
            <ProductCard v-for="product in paginatedProducts" :key="product.id" :product="product"
                @image-error="hideProduct" />
        </div>

        <nav class="pagination" aria-label="Pagination">
            <button @click="goToPage(currentPage - 1)" :disabled="currentPage <= 1">Previous</button>
            <button v-for="page in totalPages" :key="page" :class="{ active: page === currentPage }"
                @click="goToPage(page)">
                {{ page }}
            </button>
            <button @click="goToPage(currentPage + 1)" :disabled="currentPage >= totalPages">Next</button>
        </nav>
    </section>
</template>

<style scoped>
.home-page {
    max-width: 1200px;
    margin: 0 auto;
    padding: 26px 20px 40px;
}



.product-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(244px, 1fr));
    gap: 18px;
}

.pagination {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: center;
    margin: 28px 0 12px;
}

.pagination button {
    min-width: 50px;
    padding: 12px 16px;
    border: 1px solid rgba(230, 73, 140, 0.18);
    border-radius: 12px;
    background: #fff;
    color: var(--primary-3);
    cursor: pointer;
    transition: background 0.18s ease, color 0.18s ease, transform 0.18s ease;
}

.pagination button:hover:not(:disabled) {
    background: rgba(230, 73, 140, 0.12);
    transform: translateY(-1px);
}

.pagination button.active {
    background: linear-gradient(135deg, var(--primary-1), var(--primary-4));
    border-color: transparent;
    color: #fff;
}

.pagination button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}
</style>
