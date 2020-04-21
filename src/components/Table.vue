<template>
  <table class="table">
    <thead>
      <tr>
        <th></th>
        <th>Sitemap</th>
        <th>Type</th>
        <th>Submitted</th>
        <th>Last check</th>
        <th>Status</th>
        <th>URLs</th>
        <th>Recrawl sitemap</th>
        <th>Remove sitemap</th>
      </tr>
    </thead>
    <tbody>
        <Rows v-for="(item, index) in dataSitemaps" :key="index" :item="item"/>
    </tbody>
  </table>
</template>

<script>
import Rows from './Rows'

export default {
    props: {
        dataSitemaps: {
            type: Array
        }
    },
    components: {
        Rows
    },
}
</script>

<style lang="scss">
    .table {
        width: 100%;
        border-collapse: collapse;
        thead {
            background: #F6F8FE;
            tr {
                height: 50px;
                th:nth-child(2) {
                    display: flex;
                    justify-content: left;
                    align-items: center;
                    height: 50px;
                }
            }
        }
    }
</style>