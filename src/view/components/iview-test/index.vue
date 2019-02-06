<template>
  <div>
    <Card>
      <tables ref="selection" editable v-model="tableData" :columns="columns" @on-delete="handleDelete" @on-select="selectItem"/>
      <br/>
      <Page :total="dataCount" :page-size="pageSize" show-total show-elevator class="paging" @on-change="changepage"></Page>

    </Card>
  </div>
</template>

<script>
import Tables from '_c/tables'
import { getTableData } from '@/api/data'
export default {
  name: 'tables_page',
  components: {
    Tables
  },
  data () {
    return {
      columns: [
        { type: 'selection', width: 60, align: 'center' },
        { title: '序号', key: 'index', width: '120px', editable: false },
        { title: '姓名', key: 'name', sortable: true },
        { title: '邮箱', key: 'email', editable: true },
        { title: '日期', key: 'createTime' },
        {
          title: '操作',
          key: 'handle',
          options: ['delete'],
          button: [
            (h, params, vm) => {
              return h('Poptip', {
                props: {
                  confirm: true,
                  title: '你确定要删除吗?'
                },
                on: {
                  'on-ok': () => {
                    vm.$emit('on-delete', params)
                    vm.$emit('input', params.tableData.filter((item, index) => index !== params.row.initRowIndex))
                  }
                }
              }, [
                h('Button', '自定义删除')
              ])
            }
          ]
        }
      ],
      // 初始化信息总条数
      dataCount: 0,
      // 每页显示多少条
      pageSize: 6,
      tableData: [],
      totalData: []
    }
  },
  methods: {
    handleDelete (params) {
      console.log(params)
    },
    changepage (index) {
      var _start = (index - 1) * this.pageSize
      var _end = index * this.pageSize
      this.tableData = this.totalData.slice(_start, _end)
    },
    selectItem () {

    }
  },
  mounted () {
    getTableData().then(res => {
      this.totalData = res.data
      this.tableData = res.data
      this.dataCount = res.data.length
      // 初始化显示，小于每页显示条数，全显，大于每页显示条数，取前每页条数显示
      if (res.data.length < this.pageSize) {
        this.tableData = res.data
      } else {
        this.tableData = res.data.slice(0, this.pageSize)
      }
    })
  }
}
</script>

<style>

</style>
