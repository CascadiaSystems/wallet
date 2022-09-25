<template>
    <div class="access_card">
        <div class="content">
            <h1>Private Key</h1>
            <form @submit.prevent="access">
                <v-text-field
                    class="pass"
                    label="Input your private Key"
                    dense
                    solo
                    flat
                    type="password"
                    v-model="privatekey"
                    hide-details
                ></v-text-field>
                <p class="err">{{ error }}</p>
                <button
                    class="access_button"
                    @click="access"
                    :loading="isLoading"
                    :disabled="!canSubmit"
                    depressed
                >
                    Access Wallet
                </button>
            </form>
            <router-link to="/access">
                <button class="cancel_button">
                    Cancel
                </button>
            </router-link>
        </div>
    </div>
</template>
<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'
import { strip0x } from '@avalabs/avalanche-wallet-sdk'

@Component
export default class PrivateKey extends Vue {
    privatekey: string = ''
    isLoading: boolean = false
    error: string = ''
    async access() {
        if (!this.canSubmit || this.isLoading) return
        let parent = this
        this.error = ''
        this.isLoading = true
        let key = strip0x(this.privatekey)

        try {
            let res = await this.$store.dispatch('accessWalletSingleton', key)
            this.onsuccess()
        } catch (e) {
            this.onerror('Invalid Private Key.')
        }
    }
    onsuccess() {
        this.isLoading = false
        this.privatekey = ''
    }
    onerror(e: any) {
        this.error = e
        this.privatekey = ''
        this.isLoading = false
    }
    get canSubmit(): boolean {
        if (!this.privatekey) {
            return false
        }
        return true
    }
}
</script>
<style scoped lang="scss">
@use '../../main';
.pass {
    color: #3A3B3C;
    border: 1px solid #3A3B3C;
    border-radius: 0px;
    margin-bottom: 22px;
}
.access_button {
    width: 100%;
    margin-bottom: 12px;
    border-radius: 0px !important;
    padding: 5px 24px;
    min-width: 140px;
    border-radius: 6px;
    font-size: 16px;
    font-weight: 400;
    letter-spacing: 0.5px;
    text-transform: uppercase !important;
    background-color: #E4E6EB !important;
    color: #3A3B3C;
    &:hover {
        opacity: 1;
    }

    &:disabled {
        opacity: 0.8 !important;
    }
}
.access_card {
    padding: main.$container-padding;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}
.content {
    width: 340px;
    max-width: 100%;
    margin: 0px auto;
}
h1 {
    font-size: main.$m-size;
	color: var(--primary-color);
    font-weight: 400;
    margin-bottom: 24px;
}
.file_in {
    margin: 30px auto 10px;
    font-size: 13px;
    border: none !important;
    background-color: var(--bg) !important;
    /*min-width: 200px*/
}
a {
    color: main.$primary-color-light !important;
    text-decoration: none !important;
    margin: 10px 0 20px;
}
.cancel_button {
    width: 100%;
    margin-bottom: 22px;
    border-radius: 0px !important;
    padding: 5px 24px;
    min-width: 140px;
    border-radius: 0px;
    font-weight: 400;
    letter-spacing: 0.5px;
    text-transform: uppercase !important;
    border: 1px solid #E4E6EB !important;
    color: #E4E6EB;
    a {
        text-decoration:none !important;
    }
    &:hover {
        opacity: 1;
    }

    &:disabled {
        opacity: 0.8 !important;
    }
}
.remember {
    margin: 12px 0;
}
.err {
    font-size: 13px;
    color: var(--error);
    margin: 14px 0px !important;
}
@media only screen and (max-width: main.$mobile_width) {
    h1 {
        font-size: main.$m-size-mobile;
    }
    .but_primary {
        width: 100%;
    }
}
</style>
