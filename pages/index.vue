<template>
  <div class="container mx-auto px-4 py-8">
    <h1 class="text-3xl font-bold mb-6">Dashboard de Mapas e Análises</h1>

    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <!-- Mapa ocupa 2/3 da largura em telas grandes -->
      <div class="lg:col-span-2">
        <MapLibreMap
          title="Mapa do Brasil"
          @point-selected="handlePointSelection"
          ref="mapRef"
        />
      </div>

      <!-- Gráfico ocupa 1/3 da largura em telas grandes -->
      <div>
        <HighchartsGraph
          title="Análise Regional"
          :chart-type="chartType"
          :chart-data="chartData"
          :categories="chartCategories"
          ref="chartRef"
        />

        <div class="mt-4 p-4 bg-white rounded-lg shadow-lg">
          <h3 class="font-semibold mb-2">Opções de Gráfico</h3>
          <div class="flex flex-wrap gap-2">
            <button
              v-for="type in ['line', 'column', 'area', 'pie']"
              :key="type"
              @click="chartType = type"
              class="px-3 py-1 rounded text-sm"
              :class="
                chartType === type ? 'bg-blue-600 text-white' : 'bg-gray-200'
              "
            >
              {{ type }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Painel de informações -->
    <div class="mt-6 bg-white p-4 rounded-lg shadow-lg">
      <h2 class="text-xl font-semibold mb-3">Dados do Ponto Selecionado</h2>
      <div v-if="selectedPoint">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <p>
              <span class="font-medium">Latitude:</span>
              {{ selectedPoint.lat.toFixed(4) }}
            </p>
            <p>
              <span class="font-medium">Longitude:</span>
              {{ selectedPoint.lng.toFixed(4) }}
            </p>
          </div>
          <div>
            <p>
              <span class="font-medium">Região:</span>
              {{ getRegionName(selectedPoint) }}
            </p>
            <p>
              <span class="font-medium">Estimativa de População:</span>
              {{ getPopulationEstimate(selectedPoint).toLocaleString("pt-BR") }}
            </p>
          </div>
        </div>
      </div>
      <div v-else class="text-gray-500 italic">
        Clique em um ponto no mapa para ver detalhes
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

// Referências aos componentes
const mapRef = ref(null);
const chartRef = ref(null);

// Estado do gráfico
const chartType = ref("line");
const chartData = ref([10, 22, 35, 49, 68, 45, 30]);
const chartCategories = ref(["Jan", "Fev", "Mar", "Abr", "Mai", "Jun", "Jul"]);

// Estado do ponto selecionado
interface Point {
  lng: number;
  lat: number;
}
const selectedPoint = ref<Point | null>(null);

// Dados de exemplo
const samplePoints = [
  { coordinates: [-47.92, -15.78], name: "Brasília" },
  { coordinates: [-46.63, -23.55], name: "São Paulo" },
  { coordinates: [-43.17, -22.91], name: "Rio de Janeiro" },
  { coordinates: [-38.52, -3.78], name: "Fortaleza" },
  { coordinates: [-48.54, -27.59], name: "Florianópolis" },
];

onMounted(async () => {
  // Adicionar pontos de exemplo ao mapa
  if (mapRef.value) {
    const geoJson = {
      type: "FeatureCollection",
      features: samplePoints.map((point) => ({
        type: "Feature",
        properties: { name: point.name },
        geometry: {
          type: "Point",
          coordinates: point.coordinates,
        },
      })),
    };

    mapRef.value.addDataLayer(geoJson as any);
  }
});

// Manipulador de eventos
const handlePointSelection = (point: Point) => {
  selectedPoint.value = point;

  // Atualizar gráfico com dados fictícios baseados na localização
  const newData = Array.from(
    { length: 7 },
    () =>
      Math.floor(Math.random() * 50) + (Math.floor(point.lat * point.lng) % 50)
  );

  chartData.value = newData;
};

// Funções úteis
const getRegionName = (point: Point) => {
  // Lógica simples para determinar região (exemplo)
  if (point.lat > -10) return "Norte/Nordeste";
  if (point.lng < -50) return "Sul";
  if (point.lng > -43) return "Sudeste";
  return "Centro-Oeste";
};

const getPopulationEstimate = (point: Point) => {
  // Lógica fictícia para estimar população
  return Math.floor(Math.abs(point.lat * point.lng * 10000));
};
</script>
