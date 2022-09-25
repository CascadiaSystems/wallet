<template>
    <div class="mnemonic_auth">
        <div class="left">
            <header class="mnemomic_title">
                {{ $t('access.mnemonic.title') }}
            </header>
            <MnemonicPasswordInput @change="onPhraseIn"></MnemonicPasswordInput>
            <div class="button_container">
                <p class="err" v-if="err">{{ err }}</p>
                <button
                    class="access_button"
                    @click="access"
                    depressed
                    :loading="isLoading"
                    :disabled="!canSubmit"
                >
                    {{ $t('access.mnemonic.submit') }}
                </button>
                <router-link to="/access">
                    <button class="cancel_button">
                        {{ $t('access.mnemonic.cancel') }}
                    </button>
                </router-link>
            </div>
        </div>
    </div>
</template>
<script lang="ts">
import 'reflect-metadata'
import { Vue, Component, Prop } from 'vue-property-decorator'

import * as bip39 from 'bip39'
import MnemonicPasswordInput from '@/components/misc/MnemonicPasswordInput.vue'
import { CypherAES } from '@/js/CypherAES'

@Component({
    components: {
        MnemonicPasswordInput,
    },
})
export default class Mnemonic extends Vue {
    phrase: CypherAES | undefined = undefined
    isLoading: boolean = false
    err: string = ''
    canSubmit = false

    beforeDestroy() {
        this.phrase = undefined
    }

    onPhraseIn(val: string) {
        this.phrase = new CypherAES(val)
        this.formCheck()
    }

    errCheck() {
        let phrase = this.phrase?.getValue()

        if (!phrase) {
            return
        }

        let words = phrase.split(' ')

        // not a valid key phrase
        if (words.length !== 24) {
            this.err = `${this.$t('access.mnemonic.error')}`
            return false
        }

        let isValid = bip39.validateMnemonic(phrase)
        if (!isValid) {
            this.err = 'Invalid mnemonic phrase. Make sure your mnemonic is all lowercase.'
            return false
        }

        return true
    }

    getWordCount() {
        const phrase = this.phrase?.getValue() || ''
        return phrase.trim().split(' ').length
    }

    formCheck() {
        if (this.getWordCount() !== 24) {
            this.canSubmit = false
            return
        }
        this.canSubmit = true
    }

    async access() {
        const phrase = (this.phrase?.getValue() || '').trim()

        this.isLoading = true

        if (!this.errCheck()) {
            this.isLoading = false
            return
        }

        setTimeout(async () => {
            try {
                await this.$store.dispatch('accessWallet', phrase)
                this.isLoading = false
            } catch (e) {
                this.isLoading = false
                this.err = `${this.$t('access.mnemonic.error')}`
            }
        }, 500)
    }
}
</script>
<style scoped lang="scss">
@use '../../main';

.mnemonic_auth {
    margin: 0px auto;
    width: max-content;
    padding: main.$container-padding;

    .left,
    .right {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        justify-content: flex-start;
    }
    > * {
        //width: 100%;
    }
}
.mnemomic_title {
    text-align: center;
    text-transform: capitalize;
    width: 100%;
    font-size: 24px;
    font-weight: 400;
    margin-bottom: 24px;
}
h1 {
    text-align: center;
    font-size: main.$m-size;
}

textarea {
}

label {
    text-align: left;
    color: main.$primary-color-light;
    font-size: 12px;
    margin-bottom: 20px;
}

textarea,
input[type='password'] {
    margin: 20px 0;
    margin-bottom: main.$vertical-padding;
    width: 100%;
    background-color: var(--bg) !important;
    resize: none;
    min-height: 120px;
    padding: 8px 16px;
    font-size: 14px;
    color: var(--primary-color);
}

.phrase_disp {
    width: 100%;
    max-width: 560px;
    margin-bottom: main.$vertical-padding;
}

.err {
    font-size: 13px;
    color: var(--error);
    text-align: center;
    margin: 14px 0px !important;
}

.remember {
    margin-top: -20px;
    font-size: 0.75em;
}

.key_in {
    margin: 30px auto;
    margin-bottom: 6px;
    width: 100%;
    font-size: 13px;
    background-color: main.$white;
    border-radius: 4px;
}

.button_container {
    width: 100%;
    padding-top: 24px;
    padding-left: 30px;
    padding-right: 30px;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-start;
    a {
        width: 100%;
    }
}

.access_button {
    width: 100%;
    margin-bottom: 12px;
    border-radius: 0px !important;
    padding: 12px 24px;
    min-width: 140px;
    border-radius: 6px;
    font-size: 16px;
    font-weight: 400;
    letter-spacing: 0.5px;
    text-transform: uppercase !important;
    background-color: var(--primary-color-light);
    color: var(--bg);
    &:hover {
        background-color: var(--primary-color);
    }

    &:disabled {
        background-color: var(--primary-color-light) !important;
    }
}
.cancel_button {
    width: 100%;
    margin-bottom: 22px;
    border-radius: 0px !important;
    padding: 12px 24px;
    min-width: 140px;
    border-radius: 0px;
    font-weight: 400;
    letter-spacing: 0.5px;
    text-transform: uppercase !important;
    border: 1px solid var(--primary-color-light);
    color: var(--primary-color-light);
    background-color: var(--bg);
    a {
        text-decoration: none !important;
    }
    &:hover {
        border: 1px solid var(--primary-color);
        color: var(--primary-color);
    }

    &:disabled {
        border: 1px solid var(--primary-color-light) !important;
        color: var(--primary-color-light) !important;
    }
}
@include main.mobile_device {
    .mnemonic_auth {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;

        padding: main.$container-padding-mobile;

        .left,
        .right {
            flex-direction: column;
            align-items: stretch;
            justify-content: center;
        }

        .left {
            order: 2;
        }

        .right {
            order: 1;
            margin-bottom: main.$vertical-padding-mobile;
        }

        > * {
            width: 100%;
        }
    }

    h1 {
        text-align: center;
        font-size: main.$m-size-mobile;
    }

    label {
        text-align: center;
        margin-bottom: 20px;
    }

    .phrase_disp {
        width: 100%;
        max-width: 560px;
        margin-bottom: main.$vertical-padding-mobile;
    }

    .err {
        font-size: 13px;
        margin: 14px 0px !important;
    }

    .remember {
        margin-top: -20px;
        font-size: 0.75em;
    }

    .key_in {
        margin: 30px auto;
        margin-bottom: 6px;
        width: 100%;
        font-size: 13px;
    }

    .but_primary {
        margin: 0px auto;
        display: block;
        margin-top: 20px;
        margin-bottom: 15px;
    }

    .button_container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }
}
</style>
