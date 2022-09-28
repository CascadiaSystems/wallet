<template>
    <div class="node_card">
        <div>
            <label>Node ID</label>
            <p class="node_id">{{ node.nodeID }}</p>
        </div>
        <!--        <div class="meta_row"></div>-->
        <div>
            <label>Fee</label>
            <p>{{ node.fee.toFixed(2) }}%</p>
        </div>
        <!-- <div>
            <label>Uptime</label>
            <p style="font-size: 0.8rem">
                Please refer to
                <a :href="vscoutURL" target="_blank">VScout</a>
                or
                <a :href="avascanURL" target="_blank">Avascan</a>
                to get more information about a node's uptime.
            </p>
        </div> -->
        <div>
            <label>Delegators</label>
            <p>{{ node.numDelegators }}</p>
        </div>
        <!--        <div class="stake_row">-->
        <!--            -->
        <!--        </div>-->
        <div>
            <label>Active Stake</label>
            <p>{{ totalStakeBig.toLocaleString(0) }} SOPHON</p>
        </div>
        <div>
            <label>Available Stake</label>
            <p>{{ remainingStakeBig.toLocaleString(0) }} SOPHON</p>
        </div>
        <!--        <div class="dates"></div>-->
        <div class="date_row">
            <label>Stake Start Date</label>
            <p>{{ node.startTime.toLocaleDateString() }}</p>
            <p>{{ node.startTime.toLocaleTimeString() }}</p>
        </div>
        <div class="date_row">
            <label>Stake End Date</label>
            <p>
                {{ node.endTime.toLocaleDateString() }}
            </p>
            <p>{{ node.endTime.toLocaleTimeString() }}</p>
        </div>
    </div>
</template>
<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'
import { ValidatorListItem } from '@/store/modules/platform/types'
import { bnToBig } from '@/helpers/helper'
import { AvaNetwork } from '@/js/AvaNetwork'

@Component
export default class NodeCard extends Vue {
    @Prop() node!: ValidatorListItem

    get uptimeText(): string {
        return (this.node.uptime * 100).toFixed(2) + '%'
    }

    get nodeStakeBig() {
        return bnToBig(this.node.validatorStake, 9)
    }

    get delegatedStakeBig() {
        return bnToBig(this.node.delegatedStake, 9)
    }

    get remainingStakeBig() {
        return bnToBig(this.node.remainingStake, 9)
    }

    get totalStakeBig() {
        return bnToBig(this.node.validatorStake.add(this.node.delegatedStake), 9)
    }

    get avascanURL() {
        let activeNet: AvaNetwork = this.$store.state.Network.selectedNetwork

        if (activeNet.networkId === 1) {
            return `https://avascan.info/staking/validator/${this.node.nodeID}`
        } else {
            return `https://testnet.avascan.info/staking/validator/${this.node.nodeID}`
        }
    }

    get vscoutURL() {
        return `https://vscout.io/validator/${this.node.nodeID}`
    }
}
</script>
<style scoped lang="scss">
.node_card {
    //background-color: rgba(0, 0, 0, 0.02);
    background-color: var(--bg-light);
    border-radius: 0px;
    //width: max-content;

    > div {
        padding: 16px 0px;
    }
}

.node_id {
    max-width: 250px;
    word-break: break-all;
    //width: max-content;
    font-size: 14px;
    padding: 6px 0px;
    background-color: var(--bg-light);
    p {
        font-size: 16px;
        color: var(--primary-color);
    }
}

.meta_row {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    column-gap: 14px;
}
label {
    font-size: 14px;
    color: var(--primary-color-light);
}
p {
    font-size: 16px;
    color: var(--primary-color);
}

.dates {
    display: grid;
    grid-template-columns: 1fr 1fr;
    p {
        font-size: 14px;
    }
}

.stake_row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    column-gap: 8px;
}
.date_row {
    label {
        display: block;
    }
    p {
        display: inline-block;

        &:first-of-type {
            margin-right: 24px !important;
        }
    }
}
</style>
