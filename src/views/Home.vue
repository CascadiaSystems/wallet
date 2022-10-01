<template>
    <div class="home_view">
        <b-container>
            <b-row align-h="center">
                <b-col md="12" lg="10">
                    <div class="home_card">
                        <div class="menus">
                            <AccountsFound class="accounts_menu"></AccountsFound>
                            <div class="options">
                                <router-link data-cy="access" to="/access" class="menu_option">
                                    {{ $t('home.access.submit') }}
                                    <ImageDayNight
                                        day="/img/access_icons/day/check.svg"
                                        night="/img/access_icons/night/check.svg"
                                    ></ImageDayNight>
                                </router-link>
                                <router-link data-cy="create" to="/create" class="menu_option">
                                    {{ $t('home.create.submit') }}
                                    <ImageDayNight
                                        day="/img/access_icons/day/plus.svg"
                                        night="/img/access_icons/night/plus.svg"
                                    ></ImageDayNight>
                                </router-link>
                            </div>
                        </div>
                    </div>
                </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script lang="ts">
import 'reflect-metadata'
import { Vue, Component, Prop } from 'vue-property-decorator'
import ImageDayNight from '@/components/misc/ImageDayNight.vue'
import ToS from '@/components/misc/ToS.vue'

@Component({
    name: 'home',
    components: { ToS, ImageDayNight },
})
export default class Home extends Vue {}
</script>

<style scoped lang="scss">
@use "../main";
@use '/src/components/Access/menu';
.home_view {
    display: flex;
    justify-content: center;
    align-items: center;
}
.home_card {
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
    //display: grid;
    //grid-template-columns: repeat(4, 1fr);
    //grid-gap: 30px;
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
