<template>
  <div class="max-w-2xl mx-auto md:pt-5">
    <content-header>Blocks</content-header>
    <section class="bg-theme-content-background shadow-theme xl:rounded-lg py-10">
      <div class="hidden sm:block">
        <table-blocks :blocks="blocks"></table-blocks>
      </div>
      <div class="sm:hidden">
        <table-blocks-mobile :blocks="blocks"></table-blocks-mobile>
      </div>
      <paginator :start="+this.$route.params.page"></paginator>
    </section>
  </div>
</template>

<script type="text/ecmascript-6">
import ContentHeader from '@/components/ContentHeader'
import TableBlocks from '@/components/tables/Blocks'
import TableBlocksMobile from '@/components/tables/mobile/Blocks'
import Paginator from '@/components/utils/Paginator'
import BlockService from '@/services/block'

export default {
  components: {
    ContentHeader,
    TableBlocks,
    TableBlocksMobile,
    Paginator,
  },

  data: () => ({ blocks: [] }),

  created() {
    this.$on('paginatorChanged', page => this.changePage(page))
  },

  beforeRouteEnter (to, from, next) {
    BlockService
      .paginate(to.params.page)
      .then(response => next(vm => vm.setBlocks(response)))
  },

  beforeRouteUpdate (to, from, next) {
    this.blocks = []

    BlockService
      .paginate(to.params.page)
      .then(response => this.setBlocks(response))
      .then(() => next())
  },

  methods: {
    setBlocks (blocks) {
      if (!blocks) return

      this.blocks = blocks
    },

    changePage(page) {
      this.$router.push({ name: 'blocks', params: { page } })
    }
  }
}
</script>
