/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <div :class="['condition', {'condition--loader': !isCondition}]">
        <template v-if="isCondition">
            <div
                class="condition__parameters"
                :style="parametersStyle">
                <ConditionSetParameters
                    v-for="(parameter, index) in condition.parameters"
                    :key="index"
                    :parameter="parameter"
                    :item-id="itemId"
                    :item-row="itemRow" />
            </div>
            <span
                class="condition__phrase"
                v-text="conditionPhrase" />
        </template>
    </div>
</template>
<script>
import { mapState } from 'vuex';
import {
    isEmpty,
} from '~/model/objectWrapper';
import { hasOptions } from '~/model/attributes/AttributeTypes';
import ConditionSetParameters from '~/components/ConditionSetDesigner/ConditionSetParameters';

export default {
    name: 'ConditionSetItem',
    components: {
        ConditionSetParameters,
    },
    props: {
        condition: {
            type: Object,
            required: true,
        },
        itemId: {
            type: String,
            required: true,
        },
        itemRow: {
            type: Number,
            required: true,
        },
    },
    computed: {
        ...mapState('conditions', {
            conditionsValues: (state) => state.conditionsValues,
        }),
        parametersStyle() {
            const { parameters } = this.condition;
            return {
                gridTemplateColumns: `repeat(${parameters.length}, 1fr)`,
            };
        },
        isCondition() {
            return !isEmpty(this.condition);
        },
        conditionPhrase() {
            const { phrase } = this.condition;
            const placeholders = this.conditionsValues[this.itemId];

            if (!placeholders) return phrase;
            return this.replacePlaceholderOnPhrase(placeholders);
        },
    },
    methods: {
        replacePlaceholderOnPhrase(placeholders) {
            const { phrase, parameters } = this.condition;
            const findKeyWhenSelect = (clearedKey) => parameters.findIndex(
                (parameter) => parameter.name === clearedKey
                && hasOptions(parameter.type),
            );

            return phrase.replace(/\[\w+\]/g, (placeholder) => {
                const clearedKey = placeholder.slice(1).slice(0, -1);
                const foundKey = findKeyWhenSelect(clearedKey);

                if (foundKey !== -1) {
                    if (!placeholders[clearedKey]) return placeholder;
                    return placeholders[clearedKey].value;
                }
                return placeholders[clearedKey] || placeholder;
            });
        },
    },
};
</script>

<style lang="scss" scoped>
    .condition {
        display: flex;
        flex-direction: column;
        border: 1px solid $GREY;
        background-color: $WHITESMOKE;
        cursor: move;
        overflow: hidden;

        &::after {
            position: absolute;
            left: 50%;
            bottom: -8px;
            padding: 0 8px;
            transform: translateX(-50%);
            background-color: $WHITE;
            color: $GRAPHITE_DARK;
            font: $FONT_MEDIUM_14_20;
            content: "AND";
        }

        &--loader {
            border: 1px dashed $GREEN;
            background-color: $GREEN_LIGHT;
        }

        &__parameters {
            display: grid;
            grid-gap: 6%;
            grid-template-rows: 1fr;
            flex: 1;
            background-color: $WHITE;
        }

        &__phrase {
            flex: 0;
            border-top: $BORDER_DASHED_GREY;
            color: $GRAPHITE;
            font: $FONT_BOLD_12_16;
        }

        &__phrase, &__parameters {
            padding: 8px;
        }
    }
</style>
