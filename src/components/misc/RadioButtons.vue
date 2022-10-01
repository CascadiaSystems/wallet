<template>
    <div class="radio_buts">
        <button
            v-for="(key, i) in keys"
            :key="key"
            @click="select(key)"
            :active="selection === key"
            class="input_border"
        >
            {{ labels[i] }}
        </button>
    </div>
</template>
<script lang="ts">
import { Vue, Component, Prop, Model } from 'vue-property-decorator'

@Component
export default class RadioButtons extends Vue {
    @Prop() labels!: string[]
    @Prop() keys!: string[]

    @Model('change', { type: String }) readonly selection!: string

    select(val: string) {
        this.$emit('change', val)
    }
}
</script>
<style scoped lang="scss">
@use '../../main';
.radio_buts {
    display: flex;
    flex-wrap: wrap;
}
button {
    word-break: normal;
    white-space: nowrap;
    font-weight: 400;
    font-size: 14px;
    padding: 4px 12px;
    border: 1px solid transparent;
    color: var(--primary-color-light);
    background-color: var(--bg-wallet);
    margin-right: 6px;
    transition-duration: 0.2s;
    font-family: Inconsolata, monospace;

    &[active] {
        color: var(--bg-wallet);
        //border-color: #285599;
        background-color: var(--primary-color);
    }
}
.input_border {
    background-color: var(--bg);
    color: var(--primary-color-light);
}
@include main.medium-device {
    button {
        font-size: 11px;
        padding: 4px 8px;
    }
}
</style>
