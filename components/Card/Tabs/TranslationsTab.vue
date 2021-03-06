/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <ResponsiveCenteredViewTemplate :fixed="true">
        <template #header>
            <div class="language-selection-header">
                <TranslationSelect
                    v-model="selectedLanguages"
                    :options="languageOptions"
                    :solid="true"
                    :regular="true"
                    :multiselect="true"
                    :clearable="true"
                    label="Translations" />
            </div>
        </template>
        <template #content>
            <VerticalFixedScroll>
                <div class="container">
                    <VerticalCenteredView>
                        <slot :language-codes="selectedLanguageCodes" />
                    </VerticalCenteredView>
                </div>
            </VerticalFixedScroll>
        </template>
    </ResponsiveCenteredViewTemplate>
</template>

<script>
import { mapState } from 'vuex';
import ResponsiveCenteredViewTemplate from '~/core/components/Layout/Templates/ResponsiveCenteredViewTemplate';
import TranslationSelect from '~/core/components/Inputs/Select/TranslationSelect';
import VerticalFixedScroll from '~/core/components/Layout/Scroll/VerticalFixedScroll';
import VerticalCenteredView from '~/core/components/Layout/VerticalCenteredView';

export default {
    name: 'TranslationsTab',
    components: {
        VerticalCenteredView,
        VerticalFixedScroll,
        ResponsiveCenteredViewTemplate,
        TranslationSelect,
    },
    data() {
        return {
            selectedLanguages: [],
        };
    },
    created() {
        this.selectedLanguages = [
            {
                id: this.userLanguageCode,
                key: this.userLanguageCode,
                value: this.languages[this.userLanguageCode],
            },
        ];
    },
    computed: {
        ...mapState('authentication', {
            userLanguageCode: (state) => state.user.language,
        }),
        ...mapState('data', {
            languages: (state) => state.languages,
        }),
        languageOptions() {
            return Object.keys(this.languages).map((key) => ({
                id: key, key, value: this.languages[key],
            }));
        },
        selectedLanguageCodes() {
            return this.selectedLanguages.map((language) => language.id);
        },
    },
};
</script>

<style lang="scss" scoped>
    .language-selection-header {
        display: flex;
        flex: 0 0 196px;
        align-items: center;
        min-height: 55px;
    }

    .container {
        display: grid;
    }
</style>
