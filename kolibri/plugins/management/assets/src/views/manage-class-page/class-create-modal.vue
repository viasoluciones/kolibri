<template>

  <core-modal
    :title="$tr('addNewClassTitle')"
    @cancel="close"
  >
    <div>
      <form @submit.prevent="createNewClass">
        <core-textbox
          :label="$tr('classname')"
          :aria-label="$tr('classname')"
          v-model.trim="name"
          :autofocus="true"
          :required="true"
          :invalid="duplicateName"
          :error="$tr('duplicateName')"
          type="text"
        />

        <section class="footer">
          <icon-button
            class="undo-btn"
            type="button"
            :text="$tr('cancel')"
            @click="close"
          />

          <icon-button
            class="create-btn"
            type="submit"
            :text="$tr('create')"
            :primary="true"
          />
        </section>
      </form>
    </div>
  </core-modal>

</template>


<script>

  const actions = require('../../state/actions');

  module.exports = {
    $trNameSpace: 'classCreateModal',
    $trs: {
      addNewClassTitle: 'Add New Class',
      classname: 'Class Name',
      cancel: 'Cancel',
      create: 'Create',
      duplicateName: 'A class with that name already exists',
    },
    components: {
      'icon-button': require('kolibri.coreVue.components.iconButton'),
      'core-modal': require('kolibri.coreVue.components.coreModal'),
      'core-textbox': require('kolibri.coreVue.components.textbox'),
    },
    props: {
      classes: {
        type: Array,
        required: true,
      },
    },
    data() {
      return {
        name: '',
      };
    },
    computed: {
      duplicateName() {
        const index = this.classes.findIndex(
          classroom => classroom.name.toUpperCase() === this.name.toUpperCase());
        if (index === -1) {
          return false;
        }
        return true;
      },
    },
    methods: {
      createNewClass() {
        if (!this.duplicateName) {
          this.createClass(this.name);
        }
      },
      close() {
        this.displayModal(false);
      },
    },
    vuex: {
      actions: {
        createClass: actions.createClass,
        displayModal: actions.displayModal,
      },
    },
  };

</script>


<style lang="stylus" scoped>

  .footer
    text-align: center

  .create-btn, .undo-btn
    width: 48%

</style>
