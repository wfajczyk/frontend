/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <ListHeader
        :header="header"
        :is-expanded="isExpanded"
        @expand="onExpand">
        <TextField
            v-show="isSearchBtnClicked"
            :value="searchResult"
            class="search-text-field"
            solid
            small
            placeholder="Search..."
            @input="debouncedSearch"
            @focus="onSearchFocus">
            <template #append>
                <IconSearch :fill-color="searchIconFillColor" />
            </template>
        </TextField>
        <FabButton
            v-if="isSearch"
            class="search-btn"
            @click.native="onSearchBtnClick">
            <template #icon="{ fillColor }">
                <Component
                    :is="searchBtnIconComponent"
                    :fill-color="fillColor" />
            </template>
        </FabButton>
    </ListHeader>
</template>

<script>
import { debounce } from 'debounce';
import {
    GREEN, GRAPHITE, WHITE,
} from '~/assets/scss/_variables/_colors.scss';
import ListHeader from '~/core/components/List/ListHeader';
import FabButton from '~/core/components/Buttons/FabButton';
import TextField from '~/core/components/Inputs/TextField';
import IconSearch from '~/components/Icon/Actions/IconSearch';

export default {
    name: 'ListSearchHeader',
    components: {
        ListHeader,
        TextField,
        FabButton,
        IconSearch,
    },
    props: {
        header: {
            type: String,
            required: true,
        },
        isExpanded: {
            type: Boolean,
            required: true,
        },
        isSearch: {
            type: Boolean,
            required: true,
        },
    },
    data() {
        return {
            isSearchBtnClicked: false,
            isSearchFocused: false,
            searchResult: '',
        };
    },
    created() {
        this.debouncedSearch = debounce(this.onSearch, 500);
    },
    beforeDestroy() {
        delete this.debouncedSearch;
    },
    computed: {
        whiteColor() {
            return WHITE;
        },
        searchBtnIconComponent() {
            return this.isSearchBtnClicked
                ? () => import('~/components/Icon/Window/IconClose')
                : IconSearch;
        },
        searchIconFillColor() {
            return this.isSearchFocused
                ? GREEN
                : GRAPHITE;
        },
    },
    methods: {
        onExpand(isExpanded) {
            this.$emit('expand', isExpanded);
        },
        onSearch(value) {
            this.searchResult = value;
            this.$emit('searchResult', value);
        },
        onSearchBtnClick() {
            this.isSearchBtnClicked = !this.isSearchBtnClicked;

            if (!this.isSearchBtnClicked && this.searchResult !== '') {
                this.searchResult = '';
                this.onSearch(this.searchResult);
            }
        },
        onSearchFocus(isFocused) {
            this.isSearchFocused = isFocused;
        },
    },
};
</script>

<style lang="scss" scoped>
    .search-text-field {
        grid-column: 1 / 3;
        grid-row: 2;
    }

    .search-btn {
        grid-column: 2;
        grid-row: 1;
        justify-self: end;
    }
</style>
