<template>
  <v-row class="mb-2" dense>
    <v-col cols="12" md="8">
      <v-file-input
        label="Firmware binary (.bin)"
        prepend-icon="mdi-file-upload"
        accept=".bin"
        density="comfortable"
        :disabled="busy"
        @update:model-value="value => emit('firmware-input', value)"
      />
    </v-col>
    <v-col cols="12" md="4">
      <v-text-field
        :model-value="flashOffset"
        label="Flash offset"
        placeholder="0x0"
        density="comfortable"
        :disabled="busy"
        @update:model-value="value => emit('update:flashOffset', value)"
      />
    </v-col>
    <v-col cols="12" md="4">
      <v-select
        :model-value="selectedPreset"
        :items="offsetPresets"
        label="Recommended offsets"
        item-title="label"
        item-value="value"
        clearable
        density="comfortable"
        :disabled="busy"
        @update:model-value="value => handlePresetChange(value)"
      />
    </v-col>
  </v-row>

  <v-checkbox
    :model-value="eraseFlash"
    label="Erase entire flash before writing"
    density="comfortable"
    hide-details
    class="mb-4"
    :disabled="busy"
    @update:model-value="value => emit('update:eraseFlash', value)"
  />

  <v-btn
    color="primary"
    size="large"
    block
    :disabled="!canFlash || busy"
    @click="emit('flash')"
  >
    <v-icon start>mdi-lightning-bolt</v-icon>
    Flash Firmware
  </v-btn>

  <v-progress-linear
    v-if="flashInProgress"
    class="mt-4"
    :model-value="flashProgress"
    color="primary"
    height="12"
    rounded
    striped
  >
    <strong>{{ flashProgress }}%</strong>
  </v-progress-linear>
</template>

<script setup>
defineProps({
  flashOffset: {
    type: String,
    required: true,
  },
  selectedPreset: {
    type: [String, Number],
    default: null,
  },
  offsetPresets: {
    type: Array,
    default: () => [],
  },
  eraseFlash: {
    type: Boolean,
    required: true,
  },
  busy: {
    type: Boolean,
    required: true,
  },
  canFlash: {
    type: Boolean,
    required: true,
  },
  flashInProgress: {
    type: Boolean,
    required: true,
  },
  flashProgress: {
    type: Number,
    required: true,
  },
});

const emit = defineEmits([
  'update:flashOffset',
  'update:selectedPreset',
  'update:eraseFlash',
  'firmware-input',
  'flash',
  'apply-preset',
]);

function handlePresetChange(value) {
  emit('update:selectedPreset', value);
  emit('apply-preset', value);
}
</script>
