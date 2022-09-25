<template>
    <div class="chain_select">
        <button @click="setChain('X')" :active="chain === 'X'">X</button>
        <button @click="setChain('P')" :active="chain === 'P'">P</button>
        <button @click="setChain('C')" :active="chain === 'C'" v-if="isEVMSupported">C</button>
    </div>
</template>
<script lang="ts">
import 'reflect-metadata'
import { Vue, Component, Model } from 'vue-property-decorator'
import { ChainAlias, WalletType } from '@/js/wallets/types'

@Component
export default class ChainSelect extends Vue {
    @Model('change', { type: String }) readonly chain!: ChainAlias

    get isEVMSupported() {
        let wallet: WalletType | null = this.$store.state.activeWallet
        if (!wallet) return false
        return wallet.ethAddress
    }

    setChain(val: ChainAlias) {
        this.$emit('change', val)
    }
}
</script>
<style scoped lang="scss">
.chain_select {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    font-size: 13px;
    color: var(--primary-color-light);
    background-color: var(--bg-wallet);
}
button {
    padding: 12px 5px;
    outline: none !important;
    background-color: rgba(var(--bg-wallet), 0.5);
    border-top: 1px solid var(--bg-wallet-lighter);
    &:hover {
        opacity: 1;
        color: var(--secondary-color);
    }
    &[active] {
        opacity: 1;
        background-color: var(--bg-light);
        color: var(--primary-color);
        border-top-color: var(--bg-wallet-light);
        border-right: 1px solid var(--bg-wallet-lighter);
        border-left: 1px solid var(--bg-wallet-lighter);
    }
    &:first-child[active] {
        border-left: none;
    }
    &:last-child[active] {
        border-right: none;
    }
}
</style>
