<script>
export default {
  name: 'options-archetypes',
  components: {
    ArchetypeRadio: () => import('~/components/archetypes/archetype-radio.vue')

  },

  props: {
    reference: Object
  },

  data () {
    return {
      query: null
    }
  },

  apollo: {
    archetypes: {
      query: require('../../../query/archetypes/dao-archetypes.gql'),
      update: data => {
        const dao = data.getDao
        if (dao && dao.role && Array.isArray(dao.role)) {
          return dao.role
        }
        return []
      },
      variables () {
        return {
          daoId: this.$store.state.dao.docId,
          filter: { details_state_s: { regexp: '/.*approved*./i' } },
          order: { asc: 'details_title_s' }
        }
      }
    }
  },

  methods: {
    filtered (archetype) {
      if (!this.query) return true
      const needle = this.query.toLocaleLowerCase()
      return archetype && archetype.details_title_s.toLocaleLowerCase().indexOf(needle) > -1
    },
    select (archetype) {
      this.$emit('select', archetype)
      const headerName = this.$route.meta.title.split('>')
      this.$route.meta.title = `${headerName[0]} > ${headerName[1]} > ${archetype.details_title_s}`
    }
  },
  mounted () {
    if (this.reference !== null) {
      const headerName = this.$route.meta.title.split('>')
      this.$route.meta.title = `${headerName[0]} > ${headerName[1]} > ${this.reference.details_title_s}`
    }
  }
}
</script>

<template lang="pug">
.options-archetypes.q-mt-md
  .h-h4.q-py-sm.q-mt-sm Choose a role archetype and a complexity band
  q-input.q-mt-xxs.rounded-border(
        dense
        label="Filter archetypes"
        outlined
        v-model="query"
  )
  .row.q-mt-sm
    template(v-for="archetype in archetypes")
      .q-pb-sm(:class="{ 'col-4':$q.platform.is.desktop, 'q-pr-sm':$q.platform.is.desktop, 'full-width':$q.platform.is.mobile && !$q.screen.sm, 'col-6 q-px-xs':$q.screen.sm }" v-if="filtered(archetype)")
        archetype-radio(
          :archetype="archetype"
          :selected="reference && archetype.docId === reference.docId"
          @click="select(archetype)"
        )
</template>

<style lang="stylus" scoped>
</style>
