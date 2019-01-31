<template>
  <div>
    <Card shadow>
      <span>{{msg}}</span>&nbsp;&nbsp;&nbsp;&nbsp;<Button type="primary" @click="btnConfirm">确定</Button>
    </Card>
    </div>
</template>
<script>
import axios from '@/libs/api.request'
export default {
  name: 'iview_test',
  data () {
    return {
      msg: 'iview-test!!'
    }
  },
  methods: {
    btnConfirm () {
      console.log('-->iView-test btnConfirm')
    }
  },
  mounted () {
    console.log('-->init')
    axios.request({
      url: '/common/getPlantList?werks=C160',
      method: 'get'
    }).then(response => {
      console.log('---->success:', response.data.list[0].PLANT_NAME)
      // this.msg = response.data.list[0].PLANT_NAME;
    })
  }
}
</script>
