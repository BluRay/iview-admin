<template>
  <div>
    <Row :gutter="20">
        <h2>{{msg}}</h2>
    </Row>
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
