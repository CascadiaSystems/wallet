<template>
    <div class="home">
        <b-container>
            <b-row>
                <b-col>
                    <div class="access_card">
                        <div class="menus">
                            <AccountsFound class="accounts_menu"></AccountsFound>
                            <div class="options">
                                <router-link
                                    data-cy="access"
                                    to="/access"
                                    class="menu_option button_primary"
                                >
                                    {{ $t('home.access.submit') }}
                                    <ImageDayNight
                                        day="/img/access_icons/day/access.svg"
                                        night="/img/access_icons/night/access.svg"
                                    ></ImageDayNight>
                                </router-link>
                                <router-link
                                    data-cy="create"
                                    to="/create"
                                    class="menu_option button_primary"
                                >
                                    {{ $t('home.create.submit') }}
                                    <ImageDayNight
                                        day="/img/access_icons/day/create.svg"
                                        night="/img/access_icons/night/create.svg"
                                    ></ImageDayNight>
                                </router-link>
                            </div>
                        </div>

                        <ToS style="margin: 20px !important"></ToS>
                        <router-link to="/" class="link">{{ $t('access.cancel') }}</router-link>
                    </div>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script lang="ts">
import 'reflect-metadata'
import { Vue, Component, Prop } from 'vue-property-decorator'
import ToS from '@/components/misc/ToS.vue'

@Component({
    name: 'home',
    components: { ToS },
})
export default class Home extends Vue {}
</script>

<style scoped lang="scss">
@use "../main";
@use '/src/components/Access/menu';

.access_card {
    margin: 0px auto;
    display: flex;
    flex-direction: column;
    align-items: center;
}

img {
    width: main.$img-size;
    height: main.$img-size;
    margin-bottom: main.$vertical-padding;
}

h1 {
    font-size: main.$l-size;
    font-weight: 400;
}

hr {
    max-width: 67% !important;
    margin: main.$vertical-padding auto 0;
    color: main.$primary-color-light;
    opacity: 0.2;
}

.accounts_menu {
    margin-bottom: 30px;
}

.options {
    display: flex;
    flex-direction: column;
}

.menu_option {
    justify-content: space-between;
    align-items: center;
    img {
        width: 24px;
        height: 24px;
        margin: 0;
        object-fit: contain;
    }
}

.menus {
    width: 440px;
    max-width: 100%;
    margin-top: 1em;
}

@include main.mobile-device {
    img {
        width: main.$img-size-mobile;
        height: main.$img-size-mobile;
        margin-bottom: main.$vertical-padding-mobile;
    }

    h1 {
        font-size: main.$l-size-mobile;
    }

    .card {
        padding: main.$container-padding-mobile;
    }

    .options {
        display: block;
        grid-template-columns: none;
    }
}
</style>
