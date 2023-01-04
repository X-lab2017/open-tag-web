<template>
  <page-layout title="github actor id: 8517910">
    <j-form-container :disabled="formDisabled" class="detail-layout" slot="headerContent">
      <!-- 主表单区域 -->
      <a-form-model ref="form" :model="model" :rules="validatorRules" slot="detail">
        <a-row>
<!--          <a-col :span="24" >-->
<!--            <a-form-model-item label="标签对象id" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="actorId">-->
<!--              <a-input v-model="model.actorId" placeholder="请输入标签对象id" ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
<!--          <a-col :span="24" >-->
<!--            <a-form-model-item label="标签对象姓名" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="actorName">-->
<!--              <a-input v-model="model.actorName" placeholder="请输入标签对象姓名" ></a-input>-->
<!--            </a-form-model-item>-->
<!--          </a-col>-->
          <a-col :span="24" >
            <a-form-model-item label="标签对象类别标签" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="actorClass">
              <j-multi-select-tag type="checkbox" v-model="model.actorClass" dictCode="bot_lable_class" placeholder="请选择标签对象类别标签" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="标签对象标签是否难打" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="actorDifficult">
              <j-dict-select-tag type="radio" v-model="model.actorDifficult" dictCode="bot_lable_difficulty" placeholder="请选择标签对象标签是否难打" />
            </a-form-model-item>
          </a-col>
          <a-col :span="24" >
            <a-form-model-item label="标签对象是否是机器人" :labelCol="labelCol" :wrapperCol="wrapperCol" prop="actorBot">
              <j-dict-select-tag type="radio" v-model="model.actorBot" dictCode="bot_lable_bot" placeholder="请选择标签对象是否是机器人" />
            </a-form-model-item>
          </a-col>
          <a-divider />
          <a-col :span="24" >
              <a-button style="margin-bottom: 24px" type="primary">提交</a-button>
            <a-button style="margin-bottom: 24px; margin-left: 24px" type="primary">下一个</a-button>
          </a-col>
        </a-row>
      </a-form-model>
    </j-form-container>
    <a-form-model ref="form" :model="model" slot="extra">
      <a-row slot="extra" class="status-list">

          <a-col :xs="12" :sm="12">
            <div class="text">状态</div>
            <div class="heading">待审批</div>
          </a-col>
          <a-col :xs="12" :sm="12">
            <div class="text">订单金额</div>
            <div class="heading">¥ 568.08</div>
          </a-col>

      </a-row>
    </a-form-model>

    <a-card style="margin-top: 24px" :bordered="false" title="用户信息">
      <detail-list>
        <detail-list-item term="login">"LombiqBot"</detail-list-item>
        <detail-list-item term="createdAt">2014-08-21</detail-list-item>
        <detail-list-item term="location">"Budapest, Hungary"</detail-list-item>
        <detail-list-item term="company">"Lombiq Technologies Ltd."</detail-list-item>
        <detail-list-item term="bio">"I'm a friendly robot that can also pass the Turing test. (At least as far as git push/pull goes.)"</detail-list-item>
        <detail-list-item term="email">"bot@lombiq.com"</detail-list-item>
        <detail-list-item term="name">"Lombiq Bot"</detail-list-item>
      </detail-list>

      <a-card type="inner" title="统计数据">
<!--        <detail-list title="组名称" size="small">-->
        <detail-list size="small">
          <bar title="事件数量" :dataSource="barData" :height="height"/>
        </detail-list>
        <a-divider style="margin: 16px 0" />
        <detail-list title="各事件分布" size="small" :col="1">
          <pie title="各事件分布" :height="height" :data-source="pieData"/>
        </detail-list>
      </a-card>
    </a-card>

<!--    <a-card style="margin-top: 24px" :bordered="false" title="用户近半年来电记录">-->
<!--      <div class="no-data"><a-icon type="frown-o"/>暂无数据</div>-->
<!--    </a-card>-->

    <!-- 操作 -->
    <a-card
      style="margin-top: 24px"
      :bordered="false"
      :tabList="tabList"
      :activeTabKey="activeTabKey"
      @tabChange="(key) => {this.activeTabKey = key}"
    >
      <a-table
        v-if="activeTabKey === '1'"
        :columns="operationColumns"
        :dataSource="operation1"
        :pagination="false"
      >
        <template
          slot="status"
          slot-scope="status">
          <a-badge :status="status | statusTypeFilter" :text="status | statusFilter"/>
        </template>
      </a-table>
      <a-table
        v-if="activeTabKey === '2'"
        :columns="operationColumns"
        :dataSource="operation2"
        :pagination="false"
      >
        <template
          slot="status"
          slot-scope="status">
          <a-badge :status="status | statusTypeFilter" :text="status | statusFilter"/>
        </template>
      </a-table>
      <a-table
        v-if="activeTabKey === '3'"
        :columns="operationColumns"
        :dataSource="operation3"
        :pagination="false"
      >
        <template
          slot="status"
          slot-scope="status">
          <a-badge :status="status | statusTypeFilter" :text="status | statusFilter"/>
        </template>
      </a-table>
    </a-card>

  </page-layout>
</template>

<script>
  import { mixinDevice } from '@/utils/mixin.js'
  import PageLayout from '@/components/page/PageLayout'
  import DetailList from '@/components/tools/DetailList'
  import { JVxeTableModelMixin } from '@/mixins/JVxeTableModelMixin.js'
  import { JVXETypes } from '@/components/jeecg/JVxeTable'
  import { getRefPromise,VALIDATE_FAILED} from '@/components/jeecg/JVxeTable/utils/vxeUtils.js'
  import { validateDuplicateValue } from '@/utils/util'
  import JFormContainer from '@/components/jeecg/JFormContainer'
  import Bar from '@/components/chart/Bar'
  import Pie from '@/components/chart/Pie'


  const DetailListItem = DetailList.Item

  export default {
    name: "Advanced",
    components: {
      PageLayout,
      DetailList,
      DetailListItem,
      JFormContainer,
      Bar,
      Pie,
    },
    mixins: [mixinDevice,JVxeTableModelMixin],
    data () {
      return {
        height: 420,
        pieData: [],
        barData: [],
        labelCol: {
          xs: { span: 24 },
          sm: { span: 6 },
        },
        wrapperCol: {
          xs: { span: 48 },
          sm: { span: 32 },
        },
        model:{
        },
        validatorRules: {
        },
        tabList: [
          {
            key: '1',
            tab: 'comment事件日志数据'
          },
          {
            key: '2',
            tab: 'PR事件日志数据'
          },
          {
            key: '3',
            tab: 'Watch事件日志数据'
          }
        ],
        activeTabKey: '1',

        operationColumns: [
          {
            title: '操作类型',
            dataIndex: 'type',
            key: 'type'
          },
          {
            title: 'issue or pr id',
            dataIndex: 'name',
            key: 'name'
          },
          {
            title: '内容',
            dataIndex: 'status',
            key: 'status',
            // scopedSlots: { customRender: 'status' },
          },
          {
            title: '操作时间',
            dataIndex: 'updatedAt',
            key: 'updatedAt'
          },
          {
            title: '标题',
            dataIndex: 'remark',
            key: 'remark'
          }
        ],
        operation1: [
          {
            key: 'op1',
            type: 'IssueCommentEvent',
            name: '796526218',
            status: 'Hi @coderReview, I think I may have found the issue here. The only code to distinguish between a query to the asset server and the data server is this section found in the `getWebId()` method found in `datasource.js`:\n',
            updatedAt: '2017-10-03  19:23:12',
            remark: 'Query PI Points Directly'
          },
          {
            key: 'op2',
            type: 'IssueCommentEvent',
            name: '796526218',
            status: 'Sure thing! Sorry I\'m not more clear. \n' +
              '\n' +
              'So using the osisoftpi-grafana plugin in Grafana requires the creation of an Asset Framework within PI in order to query data. Basically you build your query using the Element field in the plugin to select the "path" in the AF until you find the specific attribute you would like to trend. \n' +
              '\n' +
              'We would like to be able to query data directly from the PI Point without the need to build the Asset Framework with attributes that map to the PI Points. So for instance say I have a PI Point in my Data Server called `equipment_speed` and my Data Server is simply called `myBusinessDataServer`. The actual PI Point in the Data Server would look like this:\n' +
              '\n' +
              '`\\\\myBusinessDataServer\\equipment_speed`\n' +
              '\n' +
              'I\'d like to be able to enter `\\\\myBusinessDataServer\\equipment_speed` in Grafana and trend data directly without the need to build the AF. Does that make more sense?\n' +
              '\n' +
              'It seems like the plugin quite nearly has this capability but turns itself off at some point. ',
            updatedAt: '2017-10-03  19:23:12',
            remark: 'restream'
          },
          {
            key: 'op3',
            type: 'IssueCommentEvent',
            name: '796526218',
            status: '![Snipaste_2021-01-21_19-26-32](https://user-images.githubusercontent.com/18009246/105345050-b88db100-5c1e-11eb-8c46-9f6d572b201f.png)\n' +
              '\n' +
              'here, thanks for replying',
            updatedAt: '2017-10-03  19:23:12',
            remark: 'restream'
          },
          {
            key: 'op4',
            type: 'IssueCommentEvent',
            name: '692204336',
            status: 'Oh, I found that is should not include "]", I think is not a bug, sorry about that',
            updatedAt: '2017-10-03  19:23:12',
            remark: 'Fix for #4272, #4211'
          },
          {
            key: 'op5',
            type: 'IssueCommentEvent',
            name: '777225316',
            status: 'What do you expect to promote?\n' +
              '\n' +
              'why don\'t you create an ad? [AD_Server plugin](https://github.com/WWBN/AVideo/wiki/Ad-Server-Plugin)',
            updatedAt: '2017-10-03  19:23:12',
            remark: 'Chat2 changes live screen'
          }
        ],
        operation2: [
          {
            key: 'op2',
            type: '财务复审',
            name: '付小小',
            status: 'reject',
            updatedAt: '2017-10-03  19:23:12',
            remark: '不通过原因'
          },
          {
            key: 'op3',
            type: '部门初审',
            name: '周毛毛',
            status: 'agree',
            updatedAt: '2017-10-03  19:23:12',
            remark: '-'
          },
          {
            key: 'op4',
            type: '提交订单',
            name: '林东东',
            status: 'agree',
            updatedAt: '2017-10-03  19:23:12',
            remark: '很棒'
          }
        ],
        operation3: [
          {
            key: 'op2',
            type: '财务复审',
            name: '付小小',
            status: 'reject',
            updatedAt: '2017-10-03  19:23:12',
            remark: '不通过原因'
          },
          {
            key: 'op3',
            type: '部门初审',
            name: '周毛毛',
            status: 'agree',
            updatedAt: '2017-10-03  19:23:12',
            remark: '-'
          }
        ],
      }
    },
    filters: {
      statusFilter(status) {
        const statusMap = {
          'agree': '成功',
          'reject': '驳回'
        }
        return statusMap[status]
      },
      statusTypeFilter(type) {
        const statusTypeMap = {
          'agree': 'success',
          'reject': 'error'
        }
        return statusTypeMap[type]
      }
    },
    props: {
      //表单禁用
      disabled: {
        type: Boolean,
        default: false,
        required: false
      }
    },
    computed: {
      formDisabled(){
        return this.disabled
      },
    },
    created () {
      setTimeout(() => {
        this.loadBarData()
        this.loadPieData()
      }, 100)
    },
    methods: {
      loadData(x, y, max, min, before = '', after = '月') {
        let data = []
        for (let i = 0; i < 12; i += 1) {
          data.push({
            [x]: `${before}${i + 1}${after}`,
            [y]: Math.floor(Math.random() * max) + min
          })
        }
        return data
      },
      // 加载柱状图数据
      loadBarData() {
        this.barData = this.loadData('x', 'y', 1000, 200)
      },
      loadPieData() {
        this.pieData = [
          { item: 'IssueCommentEvent', count: 40 },
          { item: 'PullRequestCommentEvent', count: 21 },
          { item: 'PullRequestReviewEvent', count: 17 },
          { item: 'IssueEvent', count: 13 },
          { item: 'others', count: 9 }
        ]
      }
    }
  }
</script>

<style lang="less" scoped>

  .detail-layout {
    margin-left: 44px;
  }
  .text {
    color: rgba(0, 0, 0, .45);
  }

  .heading {
    color: rgba(0, 0, 0, .85);
    font-size: 20px;
  }

  .no-data {
    color: rgba(0, 0, 0, .25);
    text-align: center;
    line-height: 64px;
    font-size: 16px;

    i {
      font-size: 24px;
      margin-right: 16px;
      position: relative;
      top: 3px;
    }
  }

  .mobile {
    .detail-layout {
      margin-left: unset;
    }
    .text {

    }
    .status-list {
      text-align: left;
    }
  }
</style>