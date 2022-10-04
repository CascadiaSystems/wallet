<template>
    <div class="node_selection">
        <div style="display: flex; align-items: center; justify-content: space-between">
            <div style="display: flex; align-items: center">
                <p>{{ $t('earn.delegate.list.prompt') }}:</p>
                <input
                    class="search"
                    type="text"
                    :placeholder="$t('earn.delegate.list.search')"
                    v-model="search"
                />
            </div>

            <div class="rigt_but">
                <button @click="openFilters">
                    <img src="/img/icons/filter.svg" />
                    {{ $t('earn.delegate.filter.title') }}
                </button>
            </div>
        </div>
        <ValidatorsList
            class="val_list"
            :search="search"
            @select="onselect"
            ref="val_list"
        ></ValidatorsList>
    </div>
</template>
<script lang="ts">
import 'reflect-metadata'
import { Vue, Component, Prop } from 'vue-property-decorator'

import ValidatorsList from '@/components/misc/ValidatorList/ValidatorsList.vue'
import { ValidatorListItem } from '@/store/modules/platform/types'

@Component({
    components: {
        ValidatorsList,
    },
})
export default class NodeSelection extends Vue {
    search: string = ''

    openFilters() {
        //@ts-ignore
        this.$refs.val_list.openFilters()
    }

    onselect(val: ValidatorListItem) {
        this.$emit('select', val)
    }
}
</script>
<style scoped lang="scss">
.node_selection {
    display: grid;
    overflow: auto;
    row-gap: 14px;
    grid-template-rows: max-content 1fr;
}
.val_list {
    overflow: auto;
    height: 100%;
    /*margin-top: 14px;*/
}

.search {
    padding: 4px 12px;
    border-radius: 0px;
    background-color: var(--bg);
    border: 1px solid var(--bg-wallet-lighter);
    margin-left: 12px;
    color: var(--primary-color-light);
    &::placeholder {
        color: var(--bg-wallet-lighter);
    }
}

.rigt_but {
    float: right;
    img {
        opacity: 0.7;
        &:hover {
            opacity: 1;
        }
    }
    button {
        color: var(--primary-color-light);
        display: flex;
        align-items: center;

        img {
            margin-right: 5px;
        }

        &:hover {
            color: var(--primary-color);
        }
    }
}
</style>
