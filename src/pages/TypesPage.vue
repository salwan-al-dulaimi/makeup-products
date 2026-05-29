<script setup lang="ts">
import { computed, ref } from 'vue'
import NavBar from '@/components/NavBar.vue'
import type { Product } from '@/types/product'
import products from '../../products.json'

const allProducts = products as Product[]
const hiddenIds = ref(new Set<number | string>())

const availableProducts = computed(() => {
  return allProducts.filter((product) => {
    if (!product) return false
    if (hiddenIds.value.has(product.id)) return false
    const image = (product.image_link || '').toString()
    if (!image) return false
    if (image.includes('placeholder.svg')) return false
    return true
  })
})

const groupedProductsByType = computed(() => {
  const map = new Map<string, Product[]>()

  availableProducts.value.forEach((product) => {
    const type = product.product_type?.trim() || 'unknown'
    const list = map.get(type) ?? []
    list.push(product)
    map.set(type, list)
  })

  return Array.from(map.entries()).sort(([typeA], [typeB]) => typeA.localeCompare(typeB))
})
</script>

<template>
  <section class="types-page">
    <NavBar />
    <h1>Products by Type</h1>

    <div class="summary">Total groups: {{ groupedProductsByType.length }}</div>

    <div v-if="groupedProductsByType.length">
      <ul class="type-list">
        <li class="type-item" v-for="([type, products]) in groupedProductsByType" :key="type">
          <button type="button" class="type-button">
            <span class="type-name">{{ type }}</span>
            <span class="type-count">{{ products.length }} product{{ products.length === 1 ? '' : 's' }}</span>
          </button>
        </li>
      </ul>
    </div>

    <div v-else class="empty-state">No products available.</div>
  </section>
</template>

<style scoped>
.types-page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 26px 20px 40px;
}

.summary {
  margin: 12px 0 24px;
  color: var(--text-muted);
}

.type-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: grid;
  gap: 12px;
}

.type-item {
  margin: 0;
}

.type-button {
  width: 100%;
  text-align: left;
  padding: 18px 20px;
  border: 1px solid var(--border);
  border-radius: 18px;
  background: var(--surface);
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
  color: var(--text);
  cursor: pointer;
}

.type-button:hover {
  background: var(--surface-soft);
}

.type-name {
  font-weight: 600;
}

.type-count {
  color: var(--text-muted);
}

.empty-state {
  padding: 32px 20px;
  text-align: center;
  color: var(--text-muted);
}
</style>