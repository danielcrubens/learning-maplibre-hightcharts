<template>
  <div class="relative">
    <div ref="mapContainer" class="w-full h-[600px] rounded-lg shadow-lg"></div>
    <div class="absolute top-2 left-2 bg-white p-2 rounded shadow z-10">
      <h3 class="font-bold ">{{ title }}</h3>
      <p class="text-sm text-gray-600">Clique no mapa para ver detalhes</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import maplibregl from 'maplibre-gl'

const props = defineProps({
  title: {
    type: String,
    default: 'Mapa Interativo'
  }
})

const emit = defineEmits(['point-selected'])
const mapContainer = ref<HTMLElement | null>(null)
let map: maplibregl.Map | null = null

onMounted(() => {
  if (!mapContainer.value) return

  // Inicializar o mapa
  map = new maplibregl.Map({
    container: mapContainer.value,
    style: 'https://demotiles.maplibre.org/style.json',
    center: [-47.9292, -15.7801],
    zoom: 4
  })

  // Adicionar controles
  map.addControl(new maplibregl.NavigationControl(), 'top-right')
  map.addControl(new maplibregl.ScaleControl(), 'bottom-left')

  // Evento de clique
  map.on('click', (e) => {
    const { lng, lat } = e.lngLat

    // Adicionar marcador
    new maplibregl.Marker({ color: '#3FB1CE' })
      .setLngLat([lng, lat])
      .setPopup(
        new maplibregl.Popup().setHTML(
          `<h3>Localização</h3><p>Lat: ${lat.toFixed(4)}, Lng: ${lng.toFixed(4)}</p>`
        )
      )
      .addTo(map!)

    // Emitir evento
    emit('point-selected', { lng, lat })
  })
})

onUnmounted(() => {
  if (map) {
    map.remove()
  }
})

// Expor método com segurança de carregamento de estilo
defineExpose({
  addDataLayer: (data: GeoJSON.FeatureCollection) => {
    if (!map) return

    const addLayer = () => {
      // Evita duplicar fonte e camada
      if (!map!.getSource('data-source')) {
        map!.addSource('data-source', {
          type: 'geojson',
          data: data
        })
      } else {
        // Atualiza os dados se a fonte já existir
        const source = map!.getSource('data-source') as maplibregl.GeoJSONSource
        source.setData(data)
      }

      if (!map!.getLayer('data-layer')) {
        map!.addLayer({
          id: 'data-layer',
          type: 'circle',
          source: 'data-source',
          paint: {
            'circle-radius': 6,
            'circle-color': '#B42222',
            'circle-opacity': 0.7
          }
        })
      }
    }

    // Executar de forma segura dependendo do carregamento do estilo
    if (map.isStyleLoaded()) {
      addLayer()
    } else {
      map.once('style.load', addLayer)
    }
  }
})
</script>
