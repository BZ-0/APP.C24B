<script setup>
import { ref } from "vue";

const target = ref(null);
</script>

<!-- -->
<template>
    <div
        ref="target" class="ui-indicator ui-time"
        data-transparent
        data-scheme="dynamic-transparent"
        v-bind="$attrs">
        <span class="ui-time-hour" inert>00</span>:<span class="ui-time-minute" inert>00</span>
    </div>
</template>
