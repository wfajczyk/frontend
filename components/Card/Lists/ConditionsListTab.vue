/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <VerticalTabBarListWrapper>
        <ListSearchHeader
            header="Conditions"
            :is-expanded="isExpanded"
            :is-search="false"
            @expand="onExpand" />
        <ConditionsList :language-code="userLanguageCode" />
    </VerticalTabBarListWrapper>
</template>

<script>
import { mapState } from 'vuex';

export default {
    name: 'ConditionsListTab',
    props: {
        isExpanded: {
            type: Boolean,
            required: true,
        },
    },
    components: {
        VerticalTabBarListWrapper: () => import('~/core/components/Tab/VerticalTabBarListWrapper'),
        ConditionsList: () => import('~/components/List/Conditions/ConditionsList'),
        ListSearchHeader: () => import('~/core/components/List/ListSearchHeader'),
    },
    computed: {
        ...mapState('authentication', {
            userLanguageCode: (state) => state.user.language,
        }),
    },
    methods: {
        onExpand(isExpanded) {
            this.$emit('expand', isExpanded);
        },
    },
};
</script>
