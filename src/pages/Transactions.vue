<template>
  <div class="max-w-2xl mx-auto md:pt-5">
    <content-header>Transactions</content-header>
    <section class="bg-theme-content-background shadow-theme xl:rounded-lg py-10">
      <div class="hidden sm:block">
        <table-transactions :transactions="transactions"></table-transactions>
      </div>
      <div class="sm:hidden">
        <table-transactions-mobile :transactions="transactions"></table-transactions-mobile>
      </div>
      <paginator :start="+this.$route.params.page"></paginator>
    </section>
  </div>
</template>

<script type="text/ecmascript-6">
import ContentHeader from '@/components/ContentHeader'
import TableTransactions from '@/components/tables/Transactions'
import TableTransactionsMobile from '@/components/tables/mobile/Transactions'
import Paginator from '@/components/utils/Paginator'
import TransactionService from '@/services/transaction'

export default {
  components: {
    ContentHeader,
    TableTransactions,
    TableTransactionsMobile,
    Paginator,
  },

  data: () => ({ transactions: [] }),

  created() {
    this.$on('paginatorChanged', page => this.changePage(page))
  },

  beforeRouteEnter (to, from, next) {
    TransactionService
      .paginate(to.params.page)
      .then(response => next(vm => vm.setTransactions(response)))
  },

  beforeRouteUpdate (to, from, next) {
    this.transactions = []

    TransactionService
      .paginate(to.params.page)
      .then(response => this.setTransactions(response))
      .then(() => next())
  },

  methods: {
    setTransactions (transactions) {
      if (!transactions) return

      this.transactions = transactions
    },

    changePage(page) {
      this.$router.push({ name: 'transactions', params: { page } })
    }
  }
}
</script>
