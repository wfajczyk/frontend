/*
 * Copyright © Bold Brand Commerce Sp. z o.o. All rights reserved.
 * See LICENSE for license details.
 */
<template>
    <Form>
        <FormGroup>
            <TextField
                :value="name"
                solid
                required
                regular
                label="Role name"
                :error-messages="errorNameMessage"
                :disabled="isDisabledByPrivileges"
                @input="setName" />
            <TextArea
                :value="description"
                solid
                required
                label="Role description"
                resize="none"
                :style="{height: '150px'}"
                :error-messages="errorDescMessage"
                :disabled="isDisabledByPrivileges"
                @input="setDescription" />
        </FormGroup>
    </Form>
</template>

<script>
import { mapState, mapActions } from 'vuex';
import errorValidationMixin from '~/mixins/validations/errorValidationMixin';

export default {
    name: 'UserRolesForm',
    components: {
        Form: () => import('~/core/components/Form/Form'),
        FormGroup: () => import('~/core/components/Form/FormGroup'),
        TextField: () => import('~/core/components/Inputs/TextField'),
        TextArea: () => import('~/core/components/Inputs/TextArea'),
    },
    mixins: [errorValidationMixin],
    computed: {
        ...mapState('roles', {
            roleID: (state) => state.id,
            name: (state) => state.name,
            description: (state) => state.description,
        }),
        isDisabled() {
            return Boolean(this.roleID);
        },
        isDisabledByPrivileges() {
            return (this.isDisabled && !this.$hasAccess(['USER_ROLE_UPDATE']))
            || (!this.isDisabled && !this.$hasAccess(['USER_ROLE_CREATE']));
        },
        errorNameMessage() {
            const nameIndex = 'name';
            return this.elementIsValidate(nameIndex);
        },
        errorDescMessage() {
            const descriptionIndex = 'description';
            return this.elementIsValidate(descriptionIndex);
        },
    },
    methods: {
        ...mapActions('roles', [
            'setName',
            'setDescription',
        ]),
    },
};
</script>
