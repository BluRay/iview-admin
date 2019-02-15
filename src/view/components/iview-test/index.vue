<template>
  <div>
    <Card>
      <label>姓名:</label>&nbsp;&nbsp;
      <Input v-model="search_name" placeholder="姓名..." style="width: 200px" />&nbsp;&nbsp;
      <Button type="primary" @click="btn_query">查询</Button>&nbsp;&nbsp;
      <Button type="success" @click="btn_add">新增</Button>
    </Card>
    <br/>
    <Card>
      <tables :loading="loading" ref="selection" stripe editable v-model="tableData" :columns="columns" @on-save-edit="handleSave"
      @on-delete="handleDelete" @on-select="selectItem"/>
      <br/>
      <Page :current="current" :total="dataCount" :page-size="pageSize" show-total show-elevator show-sizer class="paging"
      @on-change="changepage" @on-page-size-change="changPapeSize"></Page>
    </Card>
  </div>
</template>

<script>
import Tables from '_c/tables'
import { getTableData } from '@/api/data'
// import axios from '@/libs/api.request'
export default {
  name: 'iview_test',
  components: {
    Tables
  },
  data () {
    return {
      loading: false,
      current: 1,
      columns: [
        { type: 'selection', width: 60, align: 'center' },
        { title: '序号', key: 'index', width: '120px', editable: false },
        { title: '姓名', key: 'name', sortable: true },
        { title: '邮箱', key: 'email', editable: true },
        { title: '日期', key: 'createTime', editable: true },
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
      search_name: '',
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
      this.current = index
      this.tableData = this.totalData.slice(_start, _end)
    },
    changPapeSize (pageSize) {
      this.pageSize = pageSize
      this.changepage(1)
    },
    selectItem (selection, row) {
      console.log('-->selection', selection)
      console.log('-->row', row)
    },
    handleSave (params) {
      console.log('-->handleSave index:' + params.row.index + ',key:' + params.column.key)
      console.log('-->handleSave', params)
    },
    btn_query () {
      console.log('-->btn_query search_name :: ' + this.search_name)
      this.loading = true
      this.current = 1
      getTableData().then(res => {
        this.totalData = res.data
        this.dataCount = res.data.length
        if (res.data.length < this.pageSize) {
          this.tableData = res.data
        } else {
          this.tableData = res.data.slice(0, this.pageSize)
        }
        this.loading = false
      })
    },
    btn_add () {
      console.log('-->btn_add')
    }
  },
  mounted () {
    this.btn_query()
  }
}
</script>

<style>

</style>
