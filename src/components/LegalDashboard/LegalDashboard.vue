<script setup>
import { ref } from 'vue'
import Datepicker from '@vuepic/vue-datepicker'
import '@vuepic/vue-datepicker/dist/main.css'

// Filter data
const filters = ref({
  uf: 'Todos',
  classe: 'Todos',
  status: 'Todos',
  partePoloPassivo: 'Todos',
  advogadoPoloAtivo: 'Todos',
  orgaoJulgador: 'Todos',
  categoriaValorCausa: 'Todos'
})

// Date ranges
const dateRanges = ref({
  processo: { start: new Date('2019-01-01'), end: new Date('2024-10-31') },
  sentenca: { start: new Date('2019-01-31'), end: new Date('2024-10-31') },
  acordao: { start: new Date('2019-01-31'), end: new Date('2024-10-31') },
  tramite: { start: new Date('2019-01-31'), end: new Date('2024-10-31') },
  arquivamento: { start: new Date('2019-01-31'), end: new Date('2024-10-31') }
})

// Format date for display
const formatDate = (date) => {
  if (!date) return '';
  const day = date.getDate().toString().padStart(2, '0');
  const month = (date.getMonth() + 1).toString().padStart(2, '0');
  const year = date.getFullYear();
  return `${day}/${month}/${year}`;
}

// Statistics
const statistics = ref({
  mediaMesesAteSentenca: 12,
  mediaMesesSentencaAteTransito: 8,
  mediaMesesTransitoAteArquivamento: 9,
  valorTotalCausas: 'R$325 Bi',
  valorMedioCausas: 'R$114 Mil',
  quantidadeProcessos: '2.860.022',
  processosAtivosSemDistribuicao: '507.358',
  processosAtivosSemTramite: '244.270',
  processosAtivosComTramite: '239.417',
  processosExtintos: '1.868.977'
})

// Table data
const tableData = ref([
  { parte: 'Telefonica Brasil S.A.', quantidade: '21.229', valor: 'R$3.302.992.845' },
  { parte: 'Estado De Sao Paulo', quantidade: '21.149', valor: 'R$1.743.097.189' },
  { parte: 'Banco Bradesco S.A.', quantidade: '20.851', valor: 'R$4.975.307.858' },
  { parte: 'Fundacao Centro De Atendimento Socio-Educativo Ao Adolescente - Fundacao Casa - Sp', quantidade: '17.620', valor: 'R$703.650.745' },
  { parte: 'Alento Brasil S/A', quantidade: '13.170', valor: 'R$943.580.456' },
  { parte: 'Carrefour Comercio E Industria Ltda', quantidade: '14.995', valor: 'R$1.940.416.227' },
  { parte: 'Bt S.A.', quantidade: '14.048', valor: 'R$1.118.615.789' },
  { parte: 'Claro S.A.', quantidade: '14.026', valor: 'R$2.079.560.558' },
  { parte: 'Cooperativa Central Aurora Alimentos', quantidade: '13.548', valor: 'R$1.251.556.992' },
  { parte: 'Municipio De Guarulhos', quantidade: '13.172', valor: 'R$582.105.723' }
])

// Analysis data
const analysisBy = ref('Quantidade Processos')
const analysisUF = ref('UF')
const ufData = ref({
  'SP': 2141352,
  'PR': 278655,
  'SC': 220305
})

// Advogados data
const advogadosData = ref([
  { nome: 'Vladimir Ribeiro De Almeida', quantidade: '7.715', valor: 'R$3.672.092.517' },
  { nome: 'Renato Dias De Almeida', quantidade: '7.065', valor: 'R$1.476.761.043' },
  { nome: 'Marcus Tomas De Aquino', quantidade: '7.018', valor: 'R$4.989.564.492' },
  { nome: 'Claudemir Luis Piacini', quantidade: '4.117', valor: 'R$7.673.632.240' },
  { nome: 'Alessandro Jose Silva Lodi', quantidade: '1.960', valor: 'R$4.687.891.801' },
  { nome: 'Jefferson Leonardo Alves N De Gerard', quantidade: '847', valor: 'R$7.055.235.449' },
  { nome: 'Fernando Lopes Nascimento', quantidade: '137', valor: 'R$8.616.221.717' },
  { nome: 'Cesar Eugenio Nascimento Brasil', quantidade: '49', valor: 'R$4.602.321.055' },
  { nome: 'Clara Ferreira Dos Santos', quantidade: '24', valor: 'R$2.779.027.541' },
  { nome: 'Margarida Fernanda Barbosa', quantidade: '1', valor: 'R$5.000.000.534' }
])
</script>

<template>
  <div class="legal-dashboard">
    <!-- Header -->
    <header class="dashboard-header">
      <div class="logo">
        <img src="@/assets/logo.png" alt="Legal Trade" />
      </div>
      <div class="detalhamento">DETALHAMENTO</div>
    </header>

    <!-- Filters -->
    <div class="filters">
      <div class="filter-group">
        <label>UF</label>
        <select v-model="filters.uf">
          <option>Todos</option>
        </select>
      </div>
      <div class="filter-group">
        <label>Classe</label>
        <select v-model="filters.classe">
          <option>Todos</option>
        </select>
      </div>
      <div class="filter-group">
        <label>Status</label>
        <select v-model="filters.status">
          <option>Todos</option>
        </select>
      </div>
      <div class="filter-group">
        <label>Partes Polo Passivo</label>
        <select v-model="filters.partePoloPassivo">
          <option>Todos</option>
        </select>
      </div>
      <div class="filter-group">
        <label>Advogado Polo Ativo</label>
        <select v-model="filters.advogadoPoloAtivo">
          <option>Todos</option>
        </select>
      </div>
      <div class="filter-group">
        <label>Órgão Julgador</label>
        <select v-model="filters.orgaoJulgador">
          <option>Todos</option>
        </select>
      </div>
      <div class="filter-group">
        <label>Categoria Valor da Causa</label>
        <select v-model="filters.categoriaValorCausa">
          <option>Todos</option>
        </select>
      </div>
    </div>

    <!-- Date ranges -->
    <div class="date-ranges">
      <div class="date-group">
        <label>Data Processo</label>
        <Datepicker v-model="dateRanges.processo" range locale="pt-BR" 
                   :format="formatDate" :enable-time-picker="false" />
      </div>
      <div class="date-group">
        <label>Data Sentença</label>
        <Datepicker v-model="dateRanges.sentenca" range locale="pt-BR" 
                   :format="formatDate" :enable-time-picker="false" />
      </div>
      <div class="date-group">
        <label>Data Acórdão</label>
        <Datepicker v-model="dateRanges.acordao" range locale="pt-BR" 
                   :format="formatDate" :enable-time-picker="false" />
      </div>
      <div class="date-group">
        <label>Data Trânsito em Julgado</label>
        <Datepicker v-model="dateRanges.tramite" range locale="pt-BR" 
                   :format="formatDate" :enable-time-picker="false" />
      </div>
      <div class="date-group">
        <label>Data Arquivamento Definitivo</label>
        <Datepicker v-model="dateRanges.arquivamento" range locale="pt-BR" 
                   :format="formatDate" :enable-time-picker="false" />
      </div>
    </div>

    <!-- Statistics cards -->
    <div class="statistics-cards">
      <div class="stat-card">
        <div class="stat-title">Média Meses até Sentença</div>
        <div class="stat-value">{{ statistics.mediaMesesAteSentenca }}</div>
      </div>
      <div class="stat-card">
        <div class="stat-title">Média Meses Sentença a Trânsito em Julgado</div>
        <div class="stat-value">{{ statistics.mediaMesesSentencaAteTransito }}</div>
      </div>
      <div class="stat-card">
        <div class="stat-title">Média Meses Trânsito em Julgado e Extinção ou Arquivamento</div>
        <div class="stat-value">{{ statistics.mediaMesesTransitoAteArquivamento }}</div>
      </div>
    </div>

    <!-- Process statistics -->
    <div class="process-statistics">
      <div class="process-stat">
        <div class="stat-title">Valor Total das Causas</div>
        <div class="stat-value">{{ statistics.valorTotalCausas }}</div>
      </div>
      <div class="process-stat">
        <div class="stat-title">Valor Médio das Causas</div>
        <div class="stat-value">{{ statistics.valorMedioCausas }}</div>
      </div>
      <div class="process-stat">
        <div class="stat-title">Quantidade Processos</div>
        <div class="stat-value">{{ statistics.quantidadeProcessos }}</div>
      </div>
      <div class="process-stat">
        <div class="stat-title">Qtd Processos Ativos Não Distribuídos</div>
        <div class="stat-value">{{ statistics.processosAtivosSemDistribuicao }}</div>
      </div>
      <div class="process-stat">
        <div class="stat-title">Qtd Processos Ativos sem Trâmite em Julgado</div>
        <div class="stat-value">{{ statistics.processosAtivosSemTramite }}</div>
      </div>
      <div class="process-stat">
        <div class="stat-title">Qtd Processos Ativos com Trâmite em Julgado</div>
        <div class="stat-value">{{ statistics.processosAtivosComTramite }}</div>
      </div>
      <div class="process-stat">
        <div class="stat-title">Qtd Processos Extintos ou Arquivados</div>
        <div class="stat-value">{{ statistics.processosExtintos }}</div>
      </div>
    </div>

    <!-- Analysis section -->
    <div class="analysis-section">
      <div class="analysis-header">
        <div class="analysis-title">
          Análise de
          <select v-model="analysisBy">
            <option>Quantidade Processos</option>
          </select>
          por
          <select v-model="analysisUF">
            <option>UF</option>
          </select>
        </div>
      </div>
      
      <div class="analysis-charts">
        <div class="chart-container">
          <!-- Chart visualization would go here -->
          <div class="uf-bars">
            <div v-for="(value, key) in ufData" :key="key" class="uf-bar">
              <div class="uf-label">{{ key }}</div>
              <div class="uf-value">{{ value.toLocaleString() }}</div>
            </div>
          </div>
        </div>
        
        <!-- Data tables -->
        <div class="data-tables">
          <div class="data-table">
            <div class="table-header">
              <div>Partes Polo Passivo</div>
              <div>Quantidade Processos</div>
              <div>Valor Total das Causas</div>
            </div>
            <div class="table-body">
              <div v-for="(item, index) in tableData" :key="index" class="table-row">
                <div>{{ item.parte }}</div>
                <div>{{ item.quantidade }}</div>
                <div>{{ item.valor }}</div>
              </div>
            </div>
          </div>
          
          <div class="data-table">
            <div class="table-header">
              <div>Advogado Polo Ativo</div>
              <div>Quantidade Processos</div>
              <div>Valor Total das Causas</div>
            </div>
            <div class="table-body">
              <div v-for="(item, index) in advogadosData" :key="index" class="table-row">
                <div>{{ item.nome }}</div>
                <div>{{ item.quantidade }}</div>
                <div>{{ item.valor }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import url('./LegalDashboard.css');

/* Additional styles for datepicker */
.date-group :deep(.dp__main) {
  width: 100%;
}

.date-group :deep(.dp__input) {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
  font-size: 14px;
}

.date-group :deep(.dp__input_icon) {
  color: #0a3d62;
}

.date-group :deep(.dp__range_end),
.date-group :deep(.dp__range_start),
.date-group :deep(.dp__active_date) {
  background-color: #0a3d62;
}

.date-group :deep(.dp__today) {
  border-color: #0a3d62;
}
</style>
