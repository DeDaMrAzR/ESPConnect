<template>
  <v-expand-transition>
    <v-card v-if="chipDetails" class="mb-4" variant="tonal">
      <v-card-text>
        <v-row dense>
          <v-col cols="12" md="6">
            <div class="text-subtitle-2 text-medium-emphasis">Chip</div>
            <div class="text-body-1 font-weight-medium">
              {{ chipDetails.description || chipDetails.name }}
            </div>
            <div class="text-caption text-medium-emphasis mt-1">
              {{ chipDetails.name }}
            </div>
          </v-col>
          <v-col cols="12" md="6">
            <div class="text-subtitle-2 text-medium-emphasis">Flash</div>
            <div class="text-body-1 font-weight-medium">
              {{ chipDetails.flashSize || 'Unknown' }}
            </div>
            <div class="text-caption text-medium-emphasis mt-1">
              Crystal: {{ chipDetails.crystal || 'Unknown' }}
            </div>
            <div class="text-caption text-medium-emphasis">
              MAC: {{ chipDetails.mac || 'Unknown' }}
            </div>
          </v-col>
          <v-col cols="12">
            <div class="text-subtitle-2 text-medium-emphasis mb-2">Features</div>
            <v-chip-group column>
              <v-chip
                v-for="feature in chipDetails.features"
                :key="feature"
                class="me-2 mb-2"
                size="small"
                variant="elevated"
                color="primary"
              >
                {{ feature }}
              </v-chip>
              <v-chip v-if="!chipDetails.features?.length" size="small" variant="outlined">
                Not reported
              </v-chip>
            </v-chip-group>
          </v-col>
          <v-col v-if="chipDetails.facts?.length" cols="12">
            <div class="text-subtitle-2 text-medium-emphasis mb-3">Extra Details</div>
            <v-table density="compact" class="extra-details-table">
              <tbody>
                <tr v-for="fact in chipDetails.facts" :key="fact.label">
                  <td class="extra-details-label">
                    <div class="d-flex align-center gap-2">
                      <v-icon v-if="fact.icon" size="16">{{ fact.icon }}</v-icon>
                      <span>{{ fact.label }}</span>
                    </div>
                  </td>
                  <td class="extra-details-value">{{ fact.value }}</td>
                </tr>
              </tbody>
            </v-table>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-expand-transition>

  <v-card class="mt-6" variant="tonal">
    <v-card-title class="text-subtitle-1 font-weight-medium d-flex align-center">
      <v-icon class="me-2" size="20">mdi-monitor</v-icon>
      Session Log
      <v-spacer />
      <v-btn
        variant="text"
        color="primary"
        size="small"
        prepend-icon="mdi-trash-can-outline"
        :disabled="!logText"
        @click="emit('clear-log')"
      >
        Clear
      </v-btn>
    </v-card-title>
    <v-card-text class="log-surface">
      <pre class="log-output">{{ logText || 'Logs will appear here once actions begin.' }}</pre>
    </v-card-text>
  </v-card>
</template>

<script setup>
defineProps({
  chipDetails: {
    type: Object,
    default: null,
  },
  logText: {
    type: String,
    default: '',
  },
});

const emit = defineEmits(['clear-log']);
</script>

<style scoped>
.log-surface {
  background: rgba(15, 23, 42, 0.72);
  border-radius: 12px;
  padding: 12px;
  max-height: 320px;
  overflow-y: auto;
}

.log-output {
  margin: 0;
  font-family: 'Roboto Mono', ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono', 'Courier New', monospace;
  font-size: 0.9rem;
  line-height: 1.45;
  white-space: pre-wrap;
  color: rgba(226, 232, 240, 0.9);
}

.extra-details-table {
  border-radius: 12px;
  background: color-mix(in srgb, var(--v-theme-surface) 80%, transparent);
  border: 1px solid color-mix(in srgb, var(--v-theme-on-surface) 12%, transparent);
  overflow: hidden;
}

.extra-details-table :deep(table) {
  width: 100%;
  border-collapse: collapse;
}

.extra-details-table :deep(td) {
  padding: 10px 12px;
  border-bottom: 1px solid color-mix(in srgb, var(--v-theme-on-surface) 10%, transparent);
}

.extra-details-table :deep(tbody tr:last-child td) {
  border-bottom: none;
}

.extra-details-label {
  color: color-mix(in srgb, var(--v-theme-on-surface) 70%, transparent);
  font-size: 0.85rem;
  letter-spacing: 0.01em;
}

.extra-details-value {
  font-weight: 600;
  font-size: 0.9rem;
  text-align: right;
  color: color-mix(in srgb, var(--v-theme-on-surface) 95%, transparent);
  word-break: break-word;
}

@media (min-width: 960px) {
  .extra-details-value {
    text-align: left;
  }
}
</style>
