/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <BaseIcon
        :width="size"
        :height="size"
        :icon-color="fillColor">
        <polygon
            :transform="iconTransform"
            :points="points" />
    </BaseIcon>
</template>

<script>
import { ARROW } from '~/defaults/icons';
import { GRAPHITE } from '~/assets/scss/_variables/_colors.scss';
import BaseIcon from '~/components/Icon/BaseIcon';

export default {
    name: 'IconArrowPointer',
    components: {
        BaseIcon,
    },
    props: {
        state: {
            type: String,
            default: ARROW.LEFT,
        },
        fillColor: {
            type: String,
            default: GRAPHITE,
        },
        size: {
            type: [String, Number],
            default: '24',
        },
    },
    data() {
        return {
            points: '20 11 7.8 11 13.4 5.4 12 4 4 12 12 20 13.4 18.6 7.8 13 20 13',
        };
    },
    computed: {
        iconTransform() {
            if (this.state === ARROW.RIGHT) return 'translate(12, 12) rotate(-180) translate(-12, -12)';

            return null;
        },
    },
};
</script>
