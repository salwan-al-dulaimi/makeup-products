<script setup lang="ts">
import type { Product } from '../types/product'
const props = defineProps<{ product: Product }>()
const emit = defineEmits<{ (e: 'image-error', id: number | string): void }>()
const placeholder = '/images/placeholder.svg'
function onImgError(e: Event){
  if (!props.product || props.product.id == null) return
  emit('image-error', props.product.id)
}
</script>

<template>
  <article class="product-card">
    <router-link :to="`/product/${props.product.id}`" class="product-link">
      <div class="card-image">
        <img
          v-if="props.product.image_link"
          :src="props.product.image_link"
          :alt="props.product.name || 'product image'"
          @error="onImgError"
        />
        <div v-else class="no-image">No image</div>
      </div>

      <div class="card-body">
        <h2>{{ props.product.name }}</h2>
        <p class="brand">{{ props.product.brand || 'No brand' }}</p>
        <p class="price">
          {{ props.product.price_sign || '$' }}{{ props.product.price || 'N/A' }}
          <span v-if="props.product.currency">{{ props.product.currency }}</span>
        </p>
        <p class="meta">
          <span v-if="props.product.category">Category: {{ props.product.category }}</span>
          <span v-if="props.product.product_type"> | Type: {{ props.product.product_type }}</span>
        </p>
        <p v-if="props.product.rating != null" class="rating">Rating: {{ props.product.rating }}</p>
      </div>
    </router-link>
  </article>
</template>

<style scoped>
.product-card {
  background: var(--surface);
  border: 1px solid rgba(230, 73, 140, 0.16);
  border-radius: 20px;
  overflow: hidden;
  transition: transform 0.18s ease, box-shadow 0.18s ease;
}
.product-card:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow);
}
.product-link {
  color: inherit;
  display: grid;
}
.card-image {
  min-height: 190px;
  background: linear-gradient(180deg, rgba(230, 73, 140, 0.06) 0%, rgba(230, 109, 73, 0.04) 100%);
  display: flex;
  align-items: center;
  justify-content: center;
}
.card-image img {
  max-width: 100%;
  max-height: 180px;
  object-fit: contain;
}
.no-image {
  color: var(--text-muted);
  font-size: 0.95rem;
}
.card-body {
  padding: 18px;
}
.card-body h2 {
  font-size: 1.05rem;
  margin: 0 0 10px;
  color: var(--primary-1);
}
.brand,
.price,
.meta,
.rating {
  margin: 6px 0;
  color: var(--text-muted);
  font-size: 0.94rem;
}
.price {
  font-weight: 700;
  color: var(--primary-3);
}
.meta {
  color: #7c6e86;
}
.rating {
  color: var(--primary-5);
}
</style>
