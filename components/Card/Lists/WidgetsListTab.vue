/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <VerticalTabBarListWrapper>
        <ListHeader
            header="Widgets"
            :is-expanded="isExpanded"
            @expand="onExpand" />
        <WidgetsList />
    </VerticalTabBarListWrapper>
</template>

<script>

export default {
    name: 'WidgetsListTab',
    components: {
        VerticalTabBarListWrapper: () => import('~/core/components/Tab/VerticalTabBarListWrapper'),
        WidgetsList: () => import('~/components/List/Widgets/WidgetsList'),
        ListHeader: () => import('~/core/components/List/ListHeader'),
    },
    props: {
        isExpanded: {
            type: Boolean,
            required: true,
        },
    },
    methods: {
        onExpand(isExpanded) {
            this.$emit('expand', isExpanded);
        },
    },
};
</script>
