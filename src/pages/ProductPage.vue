<script setup lang="ts">
import { computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import type { Product } from '../types/product'
import products from '../../products.json'

const allProducts = products as Product[]
const route = useRoute()
const router = useRouter()

  const productId = computed(() => Number(route.params.id))
const product = computed(() => allProducts.find((item) => item.id === productId.value))
const goBack = () => {
  router.push({ name: 'home', query: route.query })
}
  const placeholder = '/images/placeholder.svg'
  function onImgError(){
    if (!product.value || product.value.id == null) return
    product.value.image_link = placeholder
  }
</script>

<template>
  <section class="product-page">
    <button class="back-button" @click="goBack">← Back to list</button>

    <div v-if="product" class="product-panel">
      <div class="overview-card">
        <div class="media">
          <img
            v-if="product.image_link"
            :src="product.image_link"
            :alt="product.name || 'product image'"
            @error="onImgError"
          />
          <div v-else class="no-image">No image</div>
        </div>

        <div class="overview-details">
          <h1>{{ product.name }}</h1>
          <p class="brand">{{ product.brand || 'No brand' }}</p>
          <p class="price">
            {{ product.price_sign || '$' }}{{ product.price || 'N/A' }}
            <span v-if="product.currency">{{ product.currency }}</span>
          </p>
          <p class="meta">
            <span v-if="product.category">Category: {{ product.category }}</span>
            <span v-if="product.product_type"> | Type: {{ product.product_type }}</span>
          </p>
          <p v-if="product.rating != null" class="rating">Rating: {{ product.rating }}</p>
          <p v-if="product.description" class="description">{{ product.description }}</p>
          <div class="links">
            <a v-if="product.product_link" :href="product.product_link" target="_blank" rel="noreferrer">Product link</a>
            <a v-if="product.website_link" :href="product.website_link" target="_blank" rel="noreferrer">Website link</a>
          </div>
        </div>
      </div>

      <div class="details-grid">
        <section class="detail-box">
          <h2>Additional details</h2>
          <ul>
            <li>ID: {{ product.id }}</li>
            <li v-if="product.price_sign">Currency symbol: {{ product.price_sign }}</li>
            <li v-if="product.currency">Currency: {{ product.currency }}</li>
            <li v-if="product.created_at">Created at: {{ product.created_at }}</li>
            <li v-if="product.updated_at">Updated at: {{ product.updated_at }}</li>
            <li v-if="product.product_api_url">API URL: <a :href="product.product_api_url" target="_blank" rel="noreferrer">View</a></li>
            <li v-if="product.api_featured_image">API image: <a :href="product.api_featured_image" target="_blank" rel="noreferrer">View image</a></li>
          </ul>
        </section>

        <section class="detail-box" v-if="product.tag_list?.length">
          <h2>Tags</h2>
          <div class="chip-list">
            <span class="chip" v-for="tag in product.tag_list" :key="tag">{{ tag }}</span>
          </div>
        </section>

        <section class="detail-box" v-if="product.product_colors?.length">
          <h2>Available colors</h2>
          <div class="color-list">
            <div class="color-chip" v-for="color in product.product_colors" :key="color.hex_value">
              <span class="swatch" :style="{ backgroundColor: color.hex_value }"></span>
              <span>{{ color.colour_name || color.hex_value }}</span>
            </div>
          </div>
        </section>
      </div>
    </div>

    <div v-else class="not-found">
      <h2>Product not found</h2>
      <p>The selected product is not available. Please go back to the home page.</p>
    </div>
  </section>
</template>

<style scoped>
.product-page {
  max-width: 1100px;
  margin: 0 auto;
  padding: 28px 20px 40px;
}
.back-button {
  border: 1px solid rgba(230, 73, 140, 0.24);
  background: #fff;
  border-radius: 14px;
  padding: 12px 16px;
  color: var(--primary-3);
  cursor: pointer;
  margin-bottom: 20px;
  transition: transform 0.18s ease, background 0.18s ease;
}
.back-button:hover {
  transform: translateY(-1px);
  background: rgba(230, 73, 140, 0.08);
}
.product-panel {
  display: grid;
  gap: 20px;
}
.overview-card {
  display: grid;
  grid-template-columns: minmax(260px, 1fr) 1.7fr;
  gap: 20px;
  background: var(--surface);
  border: 1px solid rgba(230, 73, 140, 0.16);
  border-radius: 24px;
  overflow: hidden;
  box-shadow: var(--shadow);
}
.media {
  background: linear-gradient(180deg, rgba(230, 105, 73, 0.12) 0%, rgba(230, 73, 140, 0.08) 100%);
  min-height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.media img {
  width: 100%;
  max-height: 100%;
  object-fit: contain;
}
.no-image {
  color: var(--text-muted);
}
.overview-details {
  padding: 26px;
}
.overview-details h1 {
  margin: 0 0 10px;
  font-size: clamp(2rem, 2.3vw, 2.3rem);
  color: var(--primary-4);
}
.brand,
.price,
.meta,
.rating,
.description {
  margin: 12px 0;
  color: var(--text-muted);
}
.price {
  font-size: 1.35rem;
  font-weight: 700;
  color: var(--primary-1);
}
.links {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  margin-top: 18px;
}
.links a {
  color: var(--primary-3);
  text-decoration: none;
  border: 1px solid rgba(230, 73, 140, 0.25);
  border-radius: 14px;
  padding: 10px 14px;
  background: rgba(230, 73, 140, 0.06);
  transition: background 0.18s ease, transform 0.18s ease;
}
.links a:hover {
  background: rgba(230, 73, 140, 0.16);
  transform: translateY(-1px);
}
.details-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 18px;
}
.detail-box {
  background: #fff;
  border: 1px solid rgba(230, 73, 140, 0.12);
  border-radius: 20px;
  padding: 20px;
}
.detail-box h2 {
  margin-top: 0;
  color: var(--primary-2);
}
.detail-box ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.detail-box li {
  padding: 8px 0;
  border-bottom: 1px solid rgba(230, 73, 140, 0.08);
}
.detail-box li:last-child {
  border-bottom: none;
}
.chip-list,
.color-list {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 14px;
}
.chip {
  background: rgba(230, 73, 140, 0.08);
  border: 1px solid rgba(230, 73, 140, 0.14);
  padding: 10px 14px;
  border-radius: 999px;
  font-size: 0.95rem;
  color: var(--primary-3);
}
.color-chip {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  border: 1px solid rgba(230, 73, 140, 0.14);
  padding: 12px;
  border-radius: 16px;
  min-width: 160px;
}
.swatch {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  border: 1px solid #d8d8d8;
}
.not-found {
  text-align: center;
  padding: 64px 18px;
  color: var(--text-muted);
}
</style>
