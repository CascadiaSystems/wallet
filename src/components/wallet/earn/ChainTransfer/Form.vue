<template>
    <div class="swap_form">
        <div>
            <label>{{ $t('cross_chain.form.source') }}</label>
            <select @input="onChangeSource" class="hover_border" v-model="sourceChain">
                <option
                    v-for="option in sourceOptions"
                    :value="option"
                    :key="option"
                    :disabled="isConfirm"
                >
                    {{ chainNames[option] }}
                </option>
            </select>
        </div>
        <div>
            <label>{{ $t('cross_chain.form.destination') }}</label>
            <p class="ledger_warn" v-if="!isEVMSupported">
                C Chain is currently not supported on Ledger devices.
            </p>
            <select @input="onChangeDestination" class="hover_border" v-model="targetChain">
                <option
                    v-for="option in destinationOptions"
                    :value="option"
                    :key="option"
                    :disabled="isConfirm"
                >
                    {{ chainNames[option] }}
                </option>
            </select>
        </div>

        <div v-if="!isConfirm">
            <label>{{ $t('earn.transfer.amount') }}</label>

            <AvaxInput
                :max="maxAmt"
                v-model="amt"
                @change="onAmtChange"
                :balance="balance"
            ></AvaxInput>
        </div>
        <div class="confirmation_val" v-else>
            <label>{{ $t('earn.transfer.amount') }}</label>
            <div class="amount">{{ formAmtText }} SOPHON</div>

            <div class="balance">$ {{ balance.toLocaleString(2) }}</div>
        </div>
    </div>
</template>
<script lang="ts">
import { Vue, Component, Prop, Watch } from 'vue-property-decorator'
import AvaxInput from '@/components/misc/AvaxInput.vue'
import { BN } from 'avalanche'
import Big from 'big.js'
import { bnToBig } from '@/helpers/helper'
import { ChainIdType } from '@/constants'
import { avm } from '@/AVA'
import MnemonicWallet from '@/js/wallets/MnemonicWallet'
import AvaAsset from '@/js/AvaAsset'
import { ChainSwapFormData } from '@/components/wallet/earn/ChainTransfer/types'
import { AvaNetwork } from '@/js/AvaNetwork'

const chainTypes: ChainIdType[] = ['X', 'P', 'C']
const chainNames = {
    X: 'X Chain',
    C: 'C Chain',
    P: 'P Chain',
}

@Component({
    components: {
        AvaxInput,
    },
})
export default class Form extends Vue {
    sourceChain: ChainIdType = 'X'
    targetChain: ChainIdType = 'P'
    amt: BN = new BN(0)

    @Prop() balance!: Big
    @Prop() maxAmt!: BN
    @Prop() isConfirm!: boolean

    clear() {
        this.amt = new BN(0)
        this.onChange()
    }

    get chainNames() {
        return chainNames
    }

    get formAmtText() {
        return bnToBig(this.amt, 9).toLocaleString()
    }

    get sourceOptions(): ChainIdType[] {
        if (!this.isEVMSupported) {
            return ['X', 'P']
        }

        let all = [...chainTypes]
        return all
    }

    get destinationOptions(): ChainIdType[] {
        return {
            X: ['P', 'C'],
            P: ['X', 'C'],
            C: ['X', 'P'],
        }[this.sourceChain] as ChainIdType[]
    }

    @Watch('destinationOptions')
    onDestinationsChange() {
        this.targetChain = this.destinationOptions[0]
        this.onChange()
    }

    get wallet() {
        let wallet: MnemonicWallet = this.$store.state.activeWallet
        return wallet
    }

    get isEVMSupported() {
        return this.wallet.ethAddress
    }

    onChangeSource(ev: any) {
        let val: ChainIdType = ev.target.value
        this.sourceChain = val
        this.onChange()
    }

    onChangeDestination(ev: any) {
        let val: ChainIdType = ev.target.value
        this.targetChain = val
        this.onChange()
    }

    onAmtChange() {
        this.onChange()
    }

    onChange() {
        let data: ChainSwapFormData = {
            sourceChain: this.sourceChain,
            destinationChain: this.targetChain,
            amount: this.amt,
        }
        this.$emit('change', data)
    }

    mounted() {
        this.onChange()
    }
}
</script>
<style scoped lang="scss">
.swap_form {
    > div {
        flex-direction: column;
        display: flex;
        margin: 13px 0;
    }
}
label {
    color: var(--primary-color);
    font-size: 15px;
    font-weight: 400;
    font-family: Roboto, sans-serif;
    margin-bottom: 4px !important;
}

select {
    width: 100%;
    color: var(--primary-color-light);
    background-color: var(--bg-wallet);
    border: 1px solid var(--bg-wallet-lighter) !important;
    padding: 11px 12px;
    font-size: 16px;
    outline: none;
    transition-duration: 0.1s;
    cursor: pointer;

    //&:hover {
    //    border-color: var(--primary-color-light);
    //}
    //
    //&:focus {
    //    border-color: var(--secondary-color);
    //}
}

.confirmation_val {
    margin-bottom: 0px !important;
    .amount {
        width: 100%;
        color: var(--primary-color-light);
        background-color: var(--bg-wallet);
        border: 1px solid var(--bg-wallet-lighter) !important;
        padding: 11px 12px;
        font-size: 16px;
        outline: none;
        transition-duration: 0.1s;
        text-align: right;
    }
    .balance {
        font-size: 14px;
        color: var(--primary-color-light);
        text-align: right;
        margin: 5px 0 !important;
    }
}

.ledger_warn {
    color: var(--info);
    font-size: 13px;
    margin-bottom: 4px !important;
}
</style>
