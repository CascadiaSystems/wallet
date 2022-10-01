<template>
    <div class="currency_select">
        <button @click="setType('SOPHON')" :active="currency === 'SOPHON'">SOPHON</button>
        <button @click="setType('USD')" :active="currency === 'USD'">USD</button>
    </div>
</template>
<script lang="ts">
import 'reflect-metadata'
import { Vue, Component, Prop, Watch, Model } from 'vue-property-decorator'
import { CurrencyType } from '@/components/misc/CurrencySelect/types'

@Component
export default class CurrencySelect extends Vue {
    @Model('change', { type: String }) readonly currency!: CurrencyType
    setType(val: CurrencyType) {
        this.$emit('change', val)
    }
}
</script>
<style scoped lang="scss">
.currency_select {
    display: flex;
    width: max-content;
    background-color: var(--bg);
    overflow: hidden;
}

button {
    margin: 0;
    border-radius: 0;
    padding: 2px 8px;
    font-size: 14px;
    color: var(--bg-wallet-lighter);
    transition-duration: 0.2s;
    width: 74px;

    &:hover {
        background-color: var(--bg-wallet-lighter);
        color: var(--bg);
    }

    &[active] {
        background-color: var(--primary-color-light);
        color: var(--bg);
    }
}
</style>
