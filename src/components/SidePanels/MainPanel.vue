<template>
    <div class="main_panel">
        <ConfirmLogout ref="logout"></ConfirmLogout>
        <div class="panel_nav">
            <DayNightToggle class="hover_but"></DayNightToggle>
            <network-menu class="net_menu"></network-menu>
            <button @click="logout" class="logout">
                {{ $t('logout.button') }}
            </button>
        </div>
        <transition name="fade" mode="out-in">
            <transaction-history-panel class="panel_content"></transaction-history-panel>
        </transition>
    </div>
</template>
<script>
import NetworkMenu from '../NetworkSettings/NetworkMenu'
import TransactionHistoryPanel from './TransactionHistoryPanel'
import DayNightToggle from '@/components/misc/DayNightToggle'
import ConfirmLogout from '@/components/modals/ConfirmLogout.vue'

export default {
    components: {
        NetworkMenu,
        TransactionHistoryPanel,
        DayNightToggle,
        ConfirmLogout,
    },
    methods: {
        logout() {
            // this.$store.dispatch('logout');
            // @ts-ignore
            this.$refs.logout.open()
        },
    },
}
</script>
<style scoped lang="scss">
@use '../../main';

.main_panel {
    display: grid;
    grid-template-rows: max-content 1fr;
    border-left: 1px solid var(--bg-wallet-lighter);
}
.panel_nav {
    background-color: var(--bg-wallet-light);
    display: flex;
    align-items: center;
    flex-direction: row;
    justify-content: space-between;
    padding: 16px;
    font-size: 16px;
    border-bottom: 1px solid var(--bg-wallet-lighter);

    > * {
        outline: none !important;
        border-radius: 0px;
        padding: 0;
    }
}

.hover_but {
    transition-duration: 0.2s;
    cursor: pointer;
    &:hover {
        box-shadow: 1px 1px 4px 1px rgba(0, 0, 0, 0.3);
    }
}

.panel_content {
    overflow: auto;
    background-color: var(--bg-wallet-light);
    height: 100%;
}

.logout {
    color: var(--primary-color-light);
    &:hover {
        color: var(--primry-color);
    }
}

@include main.medium-device {
    .panel_nav {
        padding: 12px 16px;
    }
}
</style>
