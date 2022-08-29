<template>
  <view class="flex-page">
    <at-flex wrap="wrap">
      <at-flex-item :size="12" v-for="(switchtiem, key_i) in switchValue">
        <at-switch :title="`【${name_title[key_i]}】----------- ${switchtiem ? '开启中' : '已关闭'}`" :checked="switchtiem"
          @change="handleChange(key_i + 1, $event)" />
      </at-flex-item>
    </at-flex>
  </view>
  <view class="flex-page">
    <at-flex wrap="wrap">
      <at-flex-item :size="6">
        <text class="all-data">总电压：{{ dianya }}</text>
      </at-flex-item>
      <at-flex-item :size="6">
        <text class="all-data">总电流：{{ dianliu }}</text>
      </at-flex-item>
      <at-flex-item :size="6">
        <text class="all-data">总功率：{{ gonglv }}</text>
      </at-flex-item>
      <at-flex-item :size="6">
        <text class="all-data">总电量：{{ dianliang }}</text>
      </at-flex-item>
      <at-flex-item :size="4">
        <at-button style="background-color:#13CE66;color:#fff;margin: 20px 5px;" @click="setting_ip_show()">设置
        </at-button>
      </at-flex-item>
      <at-flex-item :size="4">
        <at-button style="background-color:#FFC82C;color:#fff; margin: 20px 5px;" @click="update_date()">更新</at-button>
      </at-flex-item>
      <at-flex-item :size="4">
        <at-button style="background-color:#C2ABC7;color:#fff;margin: 20px 5px;" @click="setting_name_show()">名称设置
        </at-button>
      </at-flex-item>
    </at-flex>
  </view>
  <view>
    <at-modal :isOpened="setting_op">
      <at-modal-header>设置</at-modal-header>
      <at-modal-content>
          <picker mode='selector' :range="selector" :value="selectorValue" @change="shandleChange"
            @cancel="handleCancel">
            <view class='demo-list-item'>
              <view class='demo-list-item__label'>型号</view>
              <view class='demo-list-item__value'>
                {{ selector[selectorValue] }}
              </view>
            </view>
          </picker>
          <at-input name='acount' title='账号' type='text'  :value="state.acount"
            @change="handleInput('acount', $event)" />
          <at-input name='pswd' title='密码' type='text'  :value="state.pswd"
            @change="handleInput('pswd', $event)" />
          <at-input name='ip_address' title='IP' type='text'  :value="state.ip_address"
            @change="handleInput('ip_address', $event)" />
      </at-modal-content>
      <at-modal-action>
        <button @tap="closeModal()">取消</button>
        <button @tap="setting_ip()" style="margin-top: 0;">确定</button>
      </at-modal-action>
    </at-modal>
    <at-modal :isOpened="setting_name_op">
      <at-modal-header>开关标题设置</at-modal-header>
      <at-modal-content>
        <at-input name='name_title' title='开关1' type='text'  :value="state.form_title[0]"
          @change="handleTitleInput(0, $event)" />
        <at-input name='name_title' title='开关2' type='text'  :value="state.form_title[1]"
          @change="handleTitleInput(1, $event)" />
        <at-input name='name_title' title='开关3' type='text'  :value="state.form_title[2]"
          @change="handleTitleInput(2, $event)" />
        <at-input name='name_title' title='开关4' type='text'  :value="state.form_title[3]"
          @change="handleTitleInput(3, $event)" />
        <at-input name='name_title' title='开关5' type='text'  :value="state.form_title[4]"
          @change="handleTitleInput(4, $event)" />
        <at-input name='name_title' title='开关6' type='text'  :value="state.form_title[5]"
          @change="handleTitleInput(5, $event)" />
        <at-input name='name_title' title='开关7' type='text'  :value="state.form_title[6]"
          @change="handleTitleInput(6, $event)" />
        <at-input name='name_title' title='开关8' type='text'  :value="state.form_title[7]"
          @change="handleTitleInput(7, $event)" />
      </at-modal-content>
      <at-modal-action>
        <button @tap="closeModal()">取消</button>
        <button @tap="setting_name()" style="margin-top: 0;">确定</button>
      </at-modal-action>
    </at-modal>
  </view>
</template>
<script lang="ts">
import './index.scss'
import { ref, reactive } from 'vue'
import Taro from '@tarojs/taro'
import { CommonEvent } from '@tarojs/components/types/common'
let ip_address = ref('')
let acount = ref('')
let pswd = ref('')
let selectorValue = ref(0)
let setting_op = ref(true)
let setting_name_op = ref(true)
let name_title = ref(['开关1', '开关2', '开关3', '开关4', '开关5', '开关6', '开关7', '开关8'])
let dianliu = ref('')
let dianya = ref('')
let gonglv = ref('')
let dianliang = ref('')
let switchValue = ref([true, true, true, true, true, true, true, true])
export default {
  data () {
    return {
      selector: ['旧版810', '新版820'],
      setting_op: false,
      setting_name_op: false
    }
  },
  setup () {
    const state = reactive({
      ip_address: '',
      form_title: ['', '', '', '', '', '', '', ''],
      acount: '',
      pswd: '',
      selectorValue: 0
    })
    let title_array = name_title.value
    const shandleChange = (e: CommonEvent): void => {
      state.selectorValue = e.detail.value
      selectorValue.value = e.detail.value
    }
    const handleChange = (sw_s: number, value: boolean): void => {
      if (selectorValue.value == 1) {
        Taro.request({
          url: 'http://' + ip_address.value + '/setswitch?a=' + sw_s + '&b=' + (value ? 1 : 0) + '&'
        }).then(res => {
          console.log(res)
        })
      } else {
        Taro.request({
          url: 'http://' + ip_address.value + '/setsw?a=' + sw_s + '&b=' + (value ? 1 : 0) + '&'
        }).then(res => {
          console.log(res)
        })
      }
      switchValue.value[sw_s - 1] = value


    }
    const handleInput = (stateName: string, value: string): void => {
      state[stateName] = value
    }
    const handleTitleInput = (i: number, value: string): void => {
      title_array[i] = value
    }
    const setting_ip = (): void => {
      Taro.setStorageSync('ip_address', state.ip_address)
      Taro.setStorageSync('acount', state.acount)
      Taro.setStorageSync('pswd', state.pswd)
      Taro.setStorageSync('selectorValue', state.selectorValue)
      ip_address.value = state.ip_address
      acount.value = state.acount
      pswd.value = state.pswd
      setting_op.value = false
      console.log(ip_address)
      login_acc()
      update_date()
    }
    const setting_name = (): void => {
      Taro.setStorageSync('name_title', title_array)
      name_title.value = title_array
      setting_name_op.value = false
      console.log(ip_address)
    }
    const setting_ip_show = (): void => {
      setting_op.value = true
      state.ip_address = ip_address.value
      state.acount = acount.value
      state.pswd = pswd.value
      state.selectorValue = selectorValue.value
    }
    const setting_name_show = (): void => {
      setting_name_op.value = true
      title_array = name_title.value
      state.form_title = name_title.value
    }
    const login_acc = (): void => {
      Taro.request({
        url: 'http://' + ip_address.value + '/login?a=' + acount.value + '&b=' + pswd.value + '&'
      }).then(res => {
        console.log(res)
      })
    }
    const update_date = (): void => {
      Taro.request({
        url: 'http://' + ip_address.value + '/getstatus'
      }).then(res => {
        var stat = res.data
        var arr = stat.split("?")
        let tr = [true, true, true, true, true, true, true, true]
        let sdla = [0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00]
        if (selectorValue.value == 1) {
          for (var i = 0; i < 8; i++) {
            if (arr[i * 7 + 48] == '1') {
              tr[i] = false
            }
            if (arr[i * 7 + 48] == '2') {
              tr[i] = true
            }
            sdla[i] = arr[i * 7 + 49]
          }
          dianya.value = (arr[17] / 10).toFixed(1)
          dianliu.value = (arr[18] / 100).toFixed(2)
          gonglv.value = (arr[20] / 1000).toFixed(3)
          dianliang.value = (arr[21] / 10).toFixed(1)
        } else {
          for (var i = 0; i < 8; i++) {
            if (arr[i * 3 + 2] == '1') {
              tr[i] = false
            }
            if (arr[i * 3 + 2] == '2') {
              tr[i] = true
            }
            sdla[i] = arr[i * 3 + 3]
          }
          dianya.value = arr[76].toFixed(1)
          dianliu.value = (arr[74] / 10).toFixed(2)
          gonglv.value = ((Number(dianya.value) * Number(dianliu.value)) * 0.90).toFixed(3)
        }
        switchValue.value = tr
      })
    }
    const closeModal = () => {
      setting_op.value = false
      setting_name_op.value = false

    }
    /**
   * 用户点击右上角分享
   */
    const onShareAppMessage = () => {
      let shareData = {

      }
      return shareData
    }

    /**
     * 分享朋友圈
     * */
    const onShareTimeline = async () => {
      let shareData = {
      }
      return shareData
    }
    return {
      switchValue,
      ip_address,
      acount,
      pswd,
      dianliu,
      dianya,
      gonglv,
      dianliang,
      setting_op,
      setting_name_op,
      selectorValue,
      name_title,
      shandleChange,
      setting_ip,
      onShareAppMessage,
      onShareTimeline,
      closeModal,
      setting_ip_show,
      setting_name,
      setting_name_show,
      update_date,
      state,
      handleChange,
      handleInput,
      handleTitleInput,
      login_acc,

    }
  },
  beforeMount () {
    if (ip_address.value == null || ip_address.value == undefined || ip_address.value == '') {
      this.setting_ip_show()
    }
  },
  created () {
    setting_op.value = false
    setting_name_op.value = false
    ip_address.value = Taro.getStorageSync('ip_address')
    acount.value = Taro.getStorageSync('acount')
    pswd.value = Taro.getStorageSync('pswd')
    selectorValue.value = Taro.getStorageSync('selectorValue')
    let name_title_w = Taro.getStorageSync('name_title')
    if (name_title_w == null || name_title_w == null || name_title_w == '') {

    } else {
      name_title.value = Taro.getStorageSync('name_title')
    }
    if (ip_address.value == null || ip_address.value == undefined || ip_address.value == '') {
      ip_address.value = '192.168.1.254'
      acount.value = 'admin'
      pswd.value = 'admin'
      selectorValue.value = 0
    } else {

      //输入框赋值

      //登录
      Taro.request({
        url: 'http://' + ip_address.value + '/login?a=' + acount.value + '&b=' + pswd.value + '&'
      }).then(res => {
        console.log(res)
      })
      Taro.request({
        url: 'http://' + ip_address.value + '/getstatus'
      }).then(res => {
        var stat = res.data
        var arr = stat.split("?")
        let tr = [true, true, true, true, true, true, true, true]
        let sdla = [0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00, 0.00]
        if (selectorValue.value == 1) {
          for (var i = 0; i < 8; i++) {
            if (arr[i * 7 + 48] == '1') {
              tr[i] = false
            }
            if (arr[i * 7 + 48] == '2') {
              tr[i] = true
            }
            sdla[i] = arr[i * 7 + 49]
          }
          dianya.value = (arr[17] / 10).toFixed(1)
          dianliu.value = (arr[18] / 100).toFixed(2)
          gonglv.value = (arr[20] / 1000).toFixed(3)
          dianliang.value = (arr[21] / 10).toFixed(1)
        } else {
          for (var i = 0; i < 8; i++) {
            if (arr[i * 3 + 2] == '1') {
              tr[i] = false
            }
            if (arr[i * 3 + 2] == '2') {
              tr[i] = true
            }
            sdla[i] = arr[i * 3 + 3]
          }
          dianya.value = arr[76].toFixed(1)
          dianliu.value = (arr[74] / 10).toFixed(2)
          gonglv.value = ((Number(dianya.value) * Number(dianliu.value)) * 0.90).toFixed(3)
        }
        switchValue.value = tr
      })
    }
  }
}
</script>
