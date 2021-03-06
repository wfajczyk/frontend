/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <ElementContentBase
        :disabled="disabled"
        @mouseover.native="onMouseOver"
        @mouseout.native="onMouseOut">
        <div class="element-content__icon">
            <Component :is="attributeIconComponent" />
        </div>
        <div class="vertical-wrapper">
            <span
                :class="[typeLabelClasses, typeLabelRequireClass]"
                v-text="element.type" />
            <span
                class="element-content__subheader font--medium-14-20"
                v-text="element.label" />
        </div>
        <div
            v-if="!disabled"
            :class="['element-content__contextual-menu', contextualMenuHoveStateClasses]">
            <MenuButton
                :theme="secondaryTheme"
                :size="smallSize"
                :plain="true"
                @focus="onSelectFocus">
                <template #icon="{ fillColor }">
                    <IconDots :fill-color="fillColor" />
                </template>
                <template #content>
                    <List>
                        <ListElement
                            v-for="(option, optIndex) in contextualMenuItems"
                            :key="option"
                            @click.native="onSelectValue(optIndex)">
                            <ListElementDescription>
                                <ListElementTitle :title="option" />
                            </ListElementDescription>
                            <ListElementAction>
                                <CheckBox
                                    v-if="option === 'Required'"
                                    ref="checkbox"
                                    :value="element.required"
                                    @click.native="onSelectValue(optIndex)" />
                            </ListElementAction>
                        </ListElement>
                    </List>
                </template>
            </MenuButton>
        </div>
    </ElementContentBase>
</template>

<script>
import { mapActions } from 'vuex';
import { SIZES, THEMES } from '~/defaults/buttons';
import { capitalizeAndConcatenationArray } from '~/model/stringWrapper';
import MenuButton from '~/core/components/Buttons/MenuButton';
import IconDots from '~/components/Icon/Others/IconDots';
import CheckBox from '~/core/components/Inputs/CheckBox';
import ElementContentBase from '~/components/Template/ProductDesigner/ElementContentBase';
import List from '~/core/components/List/List';
import ListElement from '~/core/components/List/ListElement';
import ListElementAction from '~/core/components/List/ListElementAction';
import ListElementDescription from '~/core/components/List/ListElementDescription';
import ListElementTitle from '~/core/components/List/ListElementTitle';

export default {
    name: 'AttributeElementContent',
    components: {
        IconDots,
        ListElementTitle,
        MenuButton,
        ElementContentBase,
        List,
        ListElement,
        ListElementAction,
        ListElementDescription,
        CheckBox,
    },
    props: {
        index: {
            type: Number,
            required: true,
        },
        element: {
            type: Object,
            required: true,
        },
        disabled: {
            type: Boolean,
            required: true,
        },
    },
    data() {
        return {
            isContextualMenuActive: false,
            contextualMenuItems: ['Required', 'Remove'],
            isHovered: false,
        };
    },
    computed: {
        smallSize() {
            return SIZES.SMALL;
        },
        secondaryTheme() {
            return THEMES.SECONDARY;
        },
        typeLabelRequireClass() {
            return { 'element-content--required': this.element.required };
        },
        typeLabelClasses() {
            return [
                'element-content__header',
                'font--semi-bold-12-16',
            ];
        },
        attributeIconComponent() {
            if (!this.element.type) return '';

            const types = this.element.type.split('_');
            const attributeName = capitalizeAndConcatenationArray(types);

            return () => import(`~/components/Icon/Attributes/Icon${attributeName}`);
        },
        contextualMenuHoveStateClasses() {
            return { 'element-content__contextual-menu--hovered': this.isHovered };
        },
    },
    methods: {
        ...mapActions('templateDesigner', [
            'setLayoutElementRequirement',
            'removeLayoutElementAtIndex',
        ]),
        onSelectFocus(isFocused) {
            if (!isFocused) this.isHovered = false;

            this.isContextualMenuActive = isFocused;
        },
        onSelectValue(index) {
            switch (this.contextualMenuItems[index]) {
            case 'Required':
                this.setLayoutElementRequirement({
                    index: this.index,
                    required: !this.element.required,
                });
                break;
            case 'Remove':
                this.removeLayoutElementAtIndex(this.index);
                break;
            default: break;
            }
        },
        onMouseOver() {
            if (!this.isHovered) this.isHovered = true;
        },
        onMouseOut() {
            if (!this.isContextualMenuActive) this.isHovered = false;
        },
    },
};
</script>

<style lang="scss" scoped>
    .element-content {
        .vertical-wrapper {
            display: flex;
            flex: 1;
            flex-direction: column;
            padding: 6px 8px;
        }

        &__icon {
            display: flex;
            padding-top: 12px;
        }

        &__header {
            letter-spacing: 0.5px;
            color: $GRAPHITE_LIGHT;
        }

        &__subheader {
            height: 20px;
            color: $GRAPHITE_DARK;
        }

        &__header, &__subheader {
            text-overflow: ellipsis;
            overflow: hidden;
            word-break: break-all;
        }

        &__contextual-menu {
            flex: 0;
            align-items: flex-start;
            padding: 12px 0;
            opacity: 0;

            &--hovered {
                opacity: 1;
            }
        }

        &--required::after {
            position: absolute;
            color: $RED;
            content: "*";
        }
    }
</style>
