<template>
  <div class="detail-page" v-if="tableData.length">
    <!-- 基础信息 -->
    <div class="section-card">
      <div class="section-header">
        <h3>基础信息</h3>
        <p class="subtitle">报告编号与企业关键信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>报告编号</label>
          <span>{{ formatValue(tableData[0].project_enterpriseid) }}</span>
        </div>
        <div class="info-item">
          <label>企业名称</label>
          <span>{{ formatValue(tableData[0].company_name) }}</span>
        </div>
        <div class="info-item">
          <label>上传日期</label>
          <span>{{ formatDateTime(tableData2[0]?.created_at) }}</span>
        </div>
        <div class="info-item">
          <label>上传负责人</label>
          <span>{{ formatValue(tableData2[0]?.project_manager) }}</span>
        </div>
      </div>
    </div>

    <!-- 项目信息 -->
    <div class="section-card">
      <div class="section-header">
        <h3>项目信息</h3>
        <p class="subtitle">项目基本信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>项目A角</label>
          <span>{{ formatValue(tableData[0].project_a_owner) }}</span>
        </div>
        <div class="info-item">
          <label>项目B角</label>
          <span>{{ formatValue(tableData[0].project_b_owner) }}</span>
        </div>
        <div class="info-item">
          <label>市场经理</label>
          <span>{{ formatValue(tableData[0].project_market_manager) }}</span>
        </div>
        <div class="info-item">
          <label>项目来源</label>
          <span>{{ formatValue(tableData[0].project_source) }}</span>
        </div>
        <div class="info-item">
          <label>合作银行</label>
          <span>{{ formatValue(tableData[0].project_coop_bank) }}</span>
        </div>
        <div class="info-item">
          <label>申请日期</label>
          <span>{{ formatDateTime(tableData[0].project_apply_date) }}</span>
        </div>
      </div>
    </div>

    <!-- 申请贷款信息 -->
    <div class="section-card">
      <div class="section-header">
        <h3>申请贷款信息</h3>
        <p class="subtitle">贷款申请相关信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>借款人名称</label>
          <span>{{ formatValue(tableData[0].loan_borrower_name) }}</span>
        </div>
        <div class="info-item">
          <label>申请金额（万元）</label>
          <span>{{ formatValue(tableData[0].loan_apply_amount) }}</span>
        </div>
        <div class="info-item">
          <label>申请期限（月）</label>
          <span>{{ formatValue(tableData[0].loan_apply_term) }}</span>
        </div>
        <div class="info-item full-width">
          <label>贷款用途</label>
          <div class="value-text">{{ formatValue(tableData[0].loan_purpose_detail) }}</div>
        </div>
      </div>
    </div>

    <!-- 企业基本情况 -->
    <div class="section-card">
      <div class="section-header">
        <h3>企业基本情况</h3>
        <p class="subtitle">企业信息概览</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>注册资本（万元）</label>
          <span>{{ formatValue(tableData[0].company_registered_capital) }}</span>
        </div>
        <div class="info-item">
          <label>成立时间</label>
          <span>{{ formatDateTime(tableData[0].company_established_date) }}</span>
        </div>
        <div class="info-item">
          <label>注册地址</label>
          <span>{{ formatValue(tableData[0].company_registered_address) }}</span>
        </div>
        <div class="info-item">
          <label>所属行业</label>
          <span>{{ formatValue(tableData[0].business_type) }}</span>
        </div>
        <div class="info-item">
          <label>员工人数</label>
          <span>{{ formatValue(tableData[0].company_employee_count) }}</span>
        </div>
        <div class="info-item">
          <label>是否外贸型</label>
          <span>{{ formatValue(tableData[0].business_is_waimao) }}</span>
        </div>
        <div class="info-item">
          <label>是否慎入行业</label>
          <span>{{ formatValue(tableData[0].business_is_jinshen) }}</span>
        </div>
        <div class="info-item">
          <label>最近工资是否发放</label>
          <span>{{ formatValue(tableData[0].company_is_salary) }}</span>
        </div>
        <div class="info-item full-width">
          <label>主营业务</label>
          <div class="value-text">{{ formatValue(tableData[0].company_main_business) }}</div>
        </div>
        <div class="info-item full-width">
          <label>股东情况</label>
          <div class="value-text">{{ formatValue(tableData[0].company_shareholder_info) }}</div>
        </div>
      </div>
    </div>

    <!-- 实控人信息 -->
    <div class="section-card">
      <div class="section-header">
        <h3>实控人/申请人情况</h3>
        <p class="subtitle">实际控制人个人信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>实际控制人</label>
          <span>{{ formatValue(tableData[0].controller_name) }}</span>
        </div>
        <div class="info-item">
          <label>性别</label>
          <span>{{ formatValue(tableData[0].controller_gender) }}</span>
        </div>
        <div class="info-item">
          <label>籍贯</label>
          <span>{{ formatValue(tableData[0].controller_native_place) }}</span>
        </div>
        <div class="info-item">
          <label>婚姻状况</label>
          <span>{{ formatValue(tableData[0].controller_marital_status) }}</span>
        </div>
        <div class="info-item">
          <label>出生日期</label>
          <span>{{ formatDateTime(tableData[0].controller_birth_date) }}</span>
        </div>
        <div class="info-item">
          <label>文化程度</label>
          <span>{{ formatValue(tableData[0].controller_education) }}</span>
        </div>
        <div class="info-item">
          <label>从业年限</label>
          <span>{{ formatValue(tableData[0].controller_service_years) }}</span>
        </div>
        <div class="info-item">
          <label>配偶姓名</label>
          <span>{{ formatValue(tableData[0].controller_spouse_name) }}</span>
        </div>
        <div class="info-item full-width">
          <label>学习、工作及企业发展经历</label>
          <div class="value-text">{{ formatValue(tableData[0].controller_career_experience) }}</div>
        </div>
      </div>
    </div>

    <!-- 家庭与社会情况 -->
    <div class="section-card">
      <div class="section-header">
        <h3>家庭与社会情况</h3>
        <p class="subtitle">家庭及社会关系信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>家庭是否和睦</label>
          <span>{{ formatValue(tableData[0].family_is_hemu) }}</span>
        </div>
        <div class="info-item full-width">
          <label>家庭年支出</label>
          <span>{{ formatValue(tableData[0].family_annual_expense) }}</span>
        </div>
        <div class="info-item full-width">
          <label>家庭成员情况</label>
          <div class="value-text">{{ formatValue(tableData[0].family_members_info) }}</div>
        </div>
        <div class="info-item full-width">
          <label>社会关系</label>
          <div class="value-text">{{ formatValue(tableData[0].social_relationship_info) }}</div>
        </div>
      </div>
    </div>

    <!-- 居住情况 -->
    <div class="section-card">
      <div class="section-header">
        <h3>居住情况</h3>
        <p class="subtitle">住所相关信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>居住场所类型</label>
          <span>{{ formatValue(tableData[0].residence_type) }}</span>
        </div>
        <div class="info-item">
          <label>本地居住年限（年）</label>
          <span>{{ formatValue(tableData[0].residence_years) }}</span>
        </div>
        <div class="info-item full-width">
          <label>居住地址</label>
          <span>{{ formatValue(tableData[0].residence_address) }}</span>
        </div>
      </div>
    </div>

    <!-- 经营场所 -->
    <div class="section-card">
      <div class="section-header">
        <h3>经营场所</h3>
        <p class="subtitle">商业模式及场所信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item full-width">
          <label>商业模式简述</label>
          <div class="value-text">{{ formatValue(tableData[0].business_model_description) }}</div>
        </div>
      </div>
      <div v-if="tableData[0].business_sites_json" class="sub-table">
        <div class="sub-table-title">经营场所明细</div>
        <div v-for="(site, idx) in parseJSON(tableData[0].business_sites_json)" :key="idx" class="sub-item">
          <div><strong>场所{{ idx + 1 }}:</strong></div>
          <div class="sub-item-content">
            <div>地址: {{ site.address }}</div>
            <div>建筑面积（㎡）: {{ site.building_area }}</div>
            <div>土地面积（㎡）: {{ site.land_area }}</div>
            <div>所有权: {{ site.ownership }}</div>
            <div>场地月租金（万元）: {{ site.month_pay }}</div>
            <div>最近支付: {{ site.is_pay || '—' }}</div>
          </div>
        </div>
      </div>
    </div>

    <!-- 资信状况 -->
    <div class="section-card">
      <div class="section-header">
        <h3>资信状况</h3>
        <p class="subtitle">征信及账户信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>征信查询次数</label>
          <span>{{ formatValue(tableData[0].credit_inquiry_count) }}</span>
        </div>
        <div class="info-item">
          <label>逾期次数（次）</label>
          <span>{{ formatValue(tableData[0].credit_overdue_count) }}</span>
        </div>
        <div class="info-item">
          <label>最大逾期金额（万元）</label>
          <span>{{ formatValue(tableData[0].credit_max_overdue_amount) }}</span>
        </div>
        <div class="info-item full-width">
          <label>征信不良信息</label>
          <div class="value-text">{{ formatValue(tableData[0].credit_adverse_info) }}</div>
        </div>
        <div class="info-item full-width">
          <label>诉讼情况</label>
          <div class="value-text">{{ formatValue(tableData[0].litigation_status) }}</div>
        </div>
      </div>
      <div v-if="tableData[0].business_accounts_json" class="sub-table">
        <div class="sub-table-title">银行账户</div>
        <div v-for="(acc, idx) in parseJSON(tableData[0].business_accounts_json)" :key="idx" class="sub-item">
          <div><strong>账户{{ idx + 1 }}:</strong> {{ acc.account_name }} - {{ acc.account_no }}</div>
        </div>
      </div>
      <div v-if="getAccountBalanceGroups().length" class="sub-table">
        <div class="sub-table-title">银行月末余额</div>
        <div v-for="(group, gIdx) in getAccountBalanceGroups()" :key="`acc-group-${gIdx}`" class="sub-item">
          <div><strong>{{ group.label }}</strong></div>
          <div v-for="(row, idx) in group.rows" :key="`acc-row-${gIdx}-${idx}`" style="margin-left: 20px; margin-top: 4px;">
            <div><strong>{{ row.year }}年:</strong></div>
            <div style="margin-left: 12px; margin-top: 4px;">
              1月余额（万元）: {{ row.m1 }} | 2月余额（万元）: {{ row.m2 }} | 3月余额（万元）: {{ row.m3 }} | 4月余额（万元）: {{ row.m4 }} | 5月余额（万元）: {{ row.m5 }} | 6月余额（万元）: {{ row.m6 }} |
              7月余额（万元）: {{ row.m7 }} | 8月余额（万元）: {{ row.m8 }} | 9月余额（万元）: {{ row.m9 }} | 10月余额（万元）: {{ row.m10 }} | 11月余额（万元）: {{ row.m11 }} | 12月余额（万元）: {{ row.m12 }} |
              <strong>月均余额（万元）: {{ row.avg }}</strong>
            </div>
          </div>
        </div>
      </div>
      <div v-if="getAccountYearlyTotals().length" class="sub-table">
        <div class="sub-table-title">月末余额合计（按年份）</div>
        <div v-for="(row, idx) in getAccountYearlyTotals()" :key="`acc-total-${idx}`" class="sub-item">
          <div><strong>{{ row.year }}年:</strong></div>
          <div style="margin-left: 20px; margin-top: 4px;">
            1月余额合计（万元）: {{ row.m1 }} | 2月余额合计（万元）: {{ row.m2 }} | 3月余额合计（万元）: {{ row.m3 }} | 4月余额合计（万元）: {{ row.m4 }} | 5月余额合计（万元）: {{ row.m5 }} | 6月余额合计（万元）: {{ row.m6 }} |
            7月余额合计（万元）: {{ row.m7 }} | 8月余额合计（万元）: {{ row.m8 }} | 9月余额合计（万元）: {{ row.m9 }} | 10月余额合计（万元）: {{ row.m10 }} | 11月余额合计（万元）: {{ row.m11 }} | 12月余额合计（万元）: {{ row.m12 }} |
            <strong>月均余额合计（万元）: {{ row.avg }}</strong>
          </div>
        </div>
      </div>
    </div>

    <!-- 财务指标 -->
    <div class="section-card">
      <div class="section-header">
        <h3>财务指标</h3>
        <p class="subtitle">关键财务数据</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>总资产（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_fin_total_assets) }}</span>
        </div>
        <div class="info-item">
          <label>总负债（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_fin_total_liabilities) }}</span>
        </div>
        <div class="info-item">
          <label>净资产（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_fin_net_assets) }}</span>
        </div>
        <div class="info-item">
          <label>营业收入（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_fin_revenue) }}</span>
        </div>
        <div class="info-item">
          <label>净利润（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_fin_net_income) }}</span>
        </div>
        <div class="info-item">
          <label>资产负债率（%）</label>
          <span>{{ formatValue(tableData[0].analysis_ind_asset_debt_ratio) }}</span>
        </div>
        <div class="info-item">
          <label>销售负债率（%）</label>
          <span>{{ formatValue(tableData[0].analysis_ind_sales_debt_ratio) }}</span>
        </div>
        <div class="info-item">
          <label>应收账款平均账期（天）</label>
          <span>{{ formatValue(tableData[0].analysis_ind_receivable_days) }}</span>
        </div>
        <div class="info-item">
          <label>月均/日均余额（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_ind_avg_balance) }}</span>
        </div>
        <div class="info-item">
          <label>月还款/月净收益（%）</label>
          <span>{{ formatValue(tableData[0].analysis_ind_repayment_ratio) }}</span>
        </div>
        <div class="info-item full-width">
          <label>利润用途</label>
          <div class="value-text">{{ formatValue(tableData[0].analysis_profit_destination) }}</div>
        </div>
      </div>
    </div>

    <!-- 担保措施 -->
    <div class="section-card">
      <div class="section-header">
        <h3>担保措施</h3>
        <p class="subtitle">担保相关信息</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>贷款金额（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_plan_amount) }}</span>
        </div>
        <div class="info-item">
          <label>贷款期限（月）</label>
          <span>{{ formatValue(tableData[0].analysis_plan_term) }}</span>
        </div>
        <div class="info-item">
          <label>担保费率（%）</label>
          <span>{{ formatValue(tableData[0].analysis_plan_fee_rate) }}</span>
        </div>
        <div class="info-item">
          <label>企业保证</label>
          <span>{{ formatValue(tableData[0].analysis_plan_corp_guarantee) }}</span>
        </div>
        <div class="info-item">
          <label>个人保证</label>
          <span>{{ formatValue(tableData[0].analysis_plan_personal_guarantee) }}</span>
        </div>
        <div class="info-item">
          <label>是否有额外担保人</label>
          <span>{{ formatValue(tableData[0].analysis_ind_is_added_guarantor) }}</span>
        </div>
        <div class="info-item">
          <label>抵押物</label>
          <span>{{ formatValue(tableData[0].analysis_plan_collateral) }}</span>
        </div>
        <div class="info-item">
          <label>房产抵押评估值（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_plan_diyapingguzhi) }}</span>
        </div>
        <div class="info-item">
          <label>房产二押余值（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_plan_eryayuzhi) }}</span>
        </div>
        <div class="info-item">
          <label>设备抵押净值（万元）</label>
          <span>{{ formatValue(tableData[0].analysis_plan_diyajingzhi) }}</span>
        </div>
      </div>
      <div v-if="tableData[0].guarantees_json" class="sub-table">
        <div class="sub-table-title">我司在保情况</div>
        <div v-for="(g, idx) in parseJSON(tableData[0].guarantees_json)" :key="idx" class="sub-item">
          <div><strong>在保{{ idx + 1 }}:</strong> {{ g.type }} | 承保金额（万元）: {{ g.amount }} | 承保余额（万元）: {{ g.balance }} | 月还款本息（万元）: {{ g.monthly_payment }}</div>
        </div>
      </div>
      <div v-if="tableData[0].existing_loans_json" class="sub-table">
        <div class="sub-table-title">现有贷款情况</div>
        <div v-for="(loan, idx) in parseJSON(tableData[0].existing_loans_json)" :key="idx" class="sub-item">
          <div><strong>贷款{{ idx + 1 }}:</strong> {{ loan.type }} | 贷款金额（万元）: {{ loan.amount }} | 贷款余额（万元）: {{ loan.balance }} | 月还款本息（万元）: {{ loan.monthly_payment }}</div>
        </div>
      </div>
    </div>

    <!-- 其他信息 -->
    <div class="section-card">
      <div class="section-header">
        <h3>其他信息</h3>
        <p class="subtitle">附加信息及分析</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>是否成长阶段</label>
          <span>{{ formatValue(tableData[0].analysis_ind_is_growth_phase) }}</span>
        </div>
        <div class="info-item">
          <label>是否使用友贷宝</label>
          <span>{{ formatValue(tableData[0].analysis_ind_is_superior_loan) }}</span>
        </div>
        <div class="info-item full-width">
          <label>用电情况</label>
          <span>{{ formatValue(tableData[0].electricity_is_quantity === '是' ? '采集用电量' : tableData[0].electricity_is_cost === '是' ? '采集电费' : '—') }}</span>
        </div>
        <div class="info-item full-width">
          <label>用电情况说明</label>
          <div class="value-text">{{ formatValue(tableData[0].electricity_descript) }}</div>
        </div>
        <div class="info-item full-width">
          <label>软信息分析</label>
          <div class="value-text">{{ formatValue(tableData[0].analysis_soft_info) }}</div>
        </div>
        <div class="info-item full-width">
          <label>综合评价</label>
          <div class="value-text">{{ formatValue(tableData[0].analysis_summary) }}</div>
        </div>
        <div class="info-item full-width">
          <label>额度测定</label>
          <div class="value-text">{{ formatValue(tableData[0].analysis_limit_calculation) }}</div>
        </div>
        <div class="info-item full-width">
          <label>增额因素</label>
          <div class="value-text">{{ formatValue(tableData[0].analysis_limit_increase_factors) }}</div>
        </div>
      </div>
      <div v-if="tableData[0].asset_stats_json" class="sub-table">
        <div class="sub-table-title">资产统计</div>
        <div v-for="(asset, idx) in parseJSON(tableData[0].asset_stats_json)" :key="idx" class="sub-item">
          <div><strong>{{ asset.name }}:</strong> 购入价格（万元） {{ asset.buy_price }} | 当前价值（万元） {{ asset.current_value }} | 折旧（万元） {{ asset.depreciation }}</div>
        </div>
      </div>
    </div>

    <!-- 文本信息 -->
    <div class="section-card">
      <div class="section-header">
        <h3>文本信息</h3>
        <p class="subtitle">业务背景与软信息</p>
      </div>
      <div class="text-grid">
        <div class="text-block" v-for="field in textFields" :key="field.key">
          <h4>{{ field.label }}</h4>
          <div class="text-box">{{ formatValue(tableData[0][field.key]) }}</div>
        </div>
      </div>
    </div>

    <!-- 预测结果 -->
    <div class="section-card">
      <div class="section-header">
        <h3>预测结果</h3>
        <p class="subtitle">模型文本输出及专家意见</p>
      </div>
      <div class="info-grid">
        <div class="info-item">
          <label>预测额度(万元)</label>
          <span class="highlight">{{ formatValue(tableData[0].predicted) }}</span>
        </div>
        <div class="info-item">
          <label>专家测算额度(万元)</label>
          <span class="highlight">{{ formatValue(tableData[0].expert_amount) }}</span>
        </div>
      </div>
      <div class="predict-box" v-html="tableData[0].prediction_text?.replace(/\n/g, '<br>') || '—'"></div>
      <div class="expert-opinion">
        <div class="label">专家意见</div>
        <div class="value-text">{{ formatValue(tableData[0].expert_opinion) }}</div>
      </div>
    </div>

    <!-- 表格数据 -->
    <div class="section-card">
      <div class="section-header">
        <h3>表格数据</h3>
        <p class="subtitle">关键量化指标明细</p>
      </div>
      <a-table
        :columns="columns"
        :data-source="filteredTableData"
        :scroll="{ x: 'max-content' }"
        :pagination="false"
        bordered
      >
        <template #headerCell="{ column }">
          <span class="th-bold">{{ column.title }}</span>
        </template>
        <template #bodyCell="{ column, record }">
          <template v-if="column.key === 'created_at'">
            {{ formatDateTime(record.created_at) }}
          </template>
          <template v-else>
            {{ record[column.key] }}
          </template>
        </template>
      </a-table>
    </div>
  </div>
  <div v-else class="loading">加载中...</div>
</template>

<script setup>
import { ref, onMounted, computed, onActivated } from "vue";
import axios from "axios";
import { useRoute } from "vue-router";
import { API_NODE } from '@/api/config';

const route = useRoute();
const tableData = ref([]);
const tableData2 = ref([]);
const searchName = ref("");

const textFields = [
  { key: "education_work_experience", label: "学习及工作经历" },
  { key: "family_social_relations", label: "家庭成员及情况" },
  { key: "business_model", label: "商业模式" },
  { key: "counter_guarantee", label: "反担保措施" },
  { key: "main_business", label: "主营业务" },
  { key: "profit_usage", label: "近三年利润去向" },
  { key: "other_soft_info", label: "其他软信息描述" },
  { key: "loan_purpose", label: "贷款用途描述" },
];

const excludedFields = [
  "education_work_experience",
  "family_social_relations",
  "counter_guarantee",
  "business_model",
  "main_business",
  "profit_usage",
  "other_soft_info",
  "loan_purpose",
  "prediction_text",
  "id",
  "project_enterpriseid",
  "project_a_owner",
  "project_b_owner",
  "project_market_manager",
  "project_source",
  "project_coop_bank",
  "project_apply_date",
  "loan_borrower_name",
  "loan_apply_amount",
  "loan_apply_term",
  "loan_purpose_detail",
  "company_registered_capital",
  "company_established_date",
  "company_registered_address",
  "business_type",
  "company_employee_count",
  "business_is_waimao",
  "business_is_jinshen",
  "company_is_salary",
  "company_main_business",
  "company_shareholder_info",
  "controller_name",
  "controller_gender",
  "controller_native_place",
  "controller_marital_status",
  "controller_birth_date",
  "controller_education",
  "controller_service_years",
  "controller_spouse_name",
  "controller_career_experience",
  "family_is_hemu",
  "family_annual_expense",
  "family_members_info",
  "social_relationship_info",
  "residence_type",
  "residence_years",
  "residence_address",
  "business_model_description",
  "business_sites_json",
  "credit_inquiry_count",
  "credit_overdue_count",
  "credit_max_overdue_amount",
  "credit_adverse_info",
  "litigation_status",
  "business_accounts_json",
  "analysis_fin_total_assets",
  "analysis_fin_total_liabilities",
  "analysis_fin_net_assets",
  "analysis_fin_revenue",
  "analysis_fin_net_income",
  "analysis_ind_asset_debt_ratio",
  "analysis_ind_sales_debt_ratio",
  "analysis_ind_receivable_days",
  "analysis_ind_avg_balance",
  "analysis_ind_repayment_ratio",
  "analysis_plan_amount",
  "analysis_plan_term",
  "analysis_plan_fee_rate",
  "analysis_plan_corp_guarantee",
  "analysis_plan_personal_guarantee",
  "analysis_ind_is_added_guarantor",
  "analysis_plan_collateral",
  "analysis_plan_diyapingguzhi",
  "analysis_plan_eryayuzhi",
  "analysis_plan_diyajingzhi",
  "guarantees_json",
  "existing_loans_json",
  "analysis_ind_is_growth_phase",
  "analysis_ind_is_superior_loan",
  "electricity_is_quantity",
  "electricity_is_cost",
  "electricity_descript",
  "asset_stats_json",
  "analysis_soft_info",
  "analysis_summary",
  "analysis_limit_calculation",
  "analysis_limit_increase_factors",
  "predicted",
  "expert_amount",
  "expert_opinion",
  "created_by",
];

const columns = [
  { title: "公司名称", dataIndex: "company_name", key: "company_name" },
  { title: "上传日期", dataIndex: "created_at", key: "created_at" },
  { title: "申请期限", dataIndex: "application_period", key: "application_period" },
  { title: "项目经理", dataIndex: "project_manager", key: "project_manager" },
  { title: "销售负债率（%）", dataIndex: "sales_debt_ratio", key: "sales_debt_ratio" },
  { title: "资产负债率（%）", dataIndex: "asset_debt_ratio", key: "asset_debt_ratio" },
  { title: "银行流水年总额流入（万元）", dataIndex: "bank_inflow", key: "bank_inflow" },
  { title: "银行流水年总额流出（万元）", dataIndex: "bank_outflow", key: "bank_outflow" },
  { title: "销售收入（万元）", dataIndex: "annual_sales", key: "annual_sales" },
  { title: "净利润（万元）", dataIndex: "annual_net_profit", key: "annual_net_profit" },
  { title: "年净收益（万元）", dataIndex: "monthly_net_profit", key: "monthly_net_profit" },
  { title: "月均余额（万元）", dataIndex: "monthly_balance", key: "monthly_balance" },
  { title: "应收账款（万元）", dataIndex: "receivables_at_meeting", key: "receivables_at_meeting" },
  { title: "总资产（万元）", dataIndex: "total_assets", key: "total_assets" },
  { title: "总负债（万元）", dataIndex: "total_liabilities", key: "total_liabilities" },
  { title: "净资产（万元）", dataIndex: "net_assets", key: "net_assets" },
  { title: "经营性贷款", dataIndex: "monthly_repayment", key: "monthly_repayment" },
  { title: "月还款额/月净收益对比", dataIndex: "repayment_income_ratio", key: "repayment_income_ratio" },
  { title: "征信查询数", dataIndex: "credit_inquiries", key: "credit_inquiries" },
  { title: "征信逾期", dataIndex: "overdue_times", key: "overdue_times" },
  { title: "微贷产品服务方向", dataIndex: "industry_category", key: "industry_category" },
  { title: "申请额度", dataIndex: "application_amount", key: "application_amount" },
  { title: "测定额度", dataIndex: "determined_amount", key: "determined_amount" },
  { title: "报告编号", dataIndex: "project_number", key: "project_number" },
  { title: "预测结果", dataIndex: "predicted", key: "predicted" },
  { title: "专家意见", dataIndex: "expert_opinion", key: "expert_opinion" },
  { title: "专家测算额度", dataIndex: "expert_amount", key: "expert_amount" },
];

const filteredTableData = computed(() => {
  return tableData.value.map((item) => {
    const newItem = { ...item };
    excludedFields.forEach((field) => {
      delete newItem[field];
    });
    return newItem;
  });
});

const formatValue = (value) => {
  if (value === null || value === undefined || value === "") return "—";
  return value;
};

const formatDateTime = (val) => {
  if (!val) return "—";
  const d = new Date(val);
  if (Number.isNaN(d.getTime())) return val;
  return d.toLocaleString("zh-CN", { hour12: false });
};

const parseJSON = (jsonStr) => {
  try {
    if (!jsonStr) return [];
    if (Array.isArray(jsonStr)) return jsonStr;
    if (typeof jsonStr === "string") {
      const parsed = JSON.parse(jsonStr);
      return Array.isArray(parsed) ? parsed : [];
    }
    return [];
  } catch {
    return [];
  }
};

const balanceMonthKeys = Array.from({ length: 12 }, (_, index) => `m${index + 1}`);

const toFiniteNumber = (value) => {
  if (value === "" || value === null || value === undefined) {
    return null;
  }
  const parsed = Number(value);
  return Number.isFinite(parsed) ? parsed : null;
};

const formatSummaryNumber = (value) =>
  value
    .toFixed(2)
    .replace(/\.00$/, "")
    .replace(/(\.\d)0$/, "$1");

const compareYearLabel = (a, b) => {
  const aNum = Number(a);
  const bNum = Number(b);
  if (Number.isFinite(aNum) && Number.isFinite(bNum)) {
    return aNum - bNum;
  }
  return String(a).localeCompare(String(b), "zh-Hans-CN", { numeric: true });
};

const calculateAccountYearlyTotals = (rows) => {
  const totalsByYear = new Map();
  rows.forEach((row) => {
    const year = String(row?.year ?? "").trim();
    if (!year) return;
    if (!totalsByYear.has(year)) {
      totalsByYear.set(year, {
        year,
        monthlyTotals: balanceMonthKeys.reduce((acc, key) => {
          acc[key] = 0;
          return acc;
        }, {}),
        monthHasValue: balanceMonthKeys.reduce((acc, key) => {
          acc[key] = false;
          return acc;
        }, {}),
      });
    }
    const yearTotal = totalsByYear.get(year);
    balanceMonthKeys.forEach((key) => {
      const num = toFiniteNumber(row[key]);
      if (num === null) return;
      yearTotal.monthHasValue[key] = true;
      yearTotal.monthlyTotals[key] += num;
    });
  });

  return Array.from(totalsByYear.values())
    .sort((a, b) => compareYearLabel(a.year, b.year))
    .map((entry) => {
      const totalRow = { year: entry.year, avg: "" };
      let hasAllMonths = true;
      let yearlySum = 0;
      let hasAnyMonth = false;
      balanceMonthKeys.forEach((key) => {
        if (!entry.monthHasValue[key]) {
          totalRow[key] = "";
          hasAllMonths = false;
          return;
        }
        hasAnyMonth = true;
        const monthTotal = entry.monthlyTotals[key];
        totalRow[key] = formatSummaryNumber(monthTotal);
        yearlySum += monthTotal;
      });
      if (!hasAnyMonth) {
        return null;
      }
      if (hasAllMonths) {
        totalRow.avg = formatSummaryNumber(yearlySum / balanceMonthKeys.length);
      }
      return totalRow;
    })
    .filter((row) => row !== null);
};

const getAccountBalanceGroups = () => {
  const record = tableData.value[0] || {};
  const accounts = parseJSON(record.business_accounts_json).map((acc, idx) => ({
    ...acc,
    account_id: acc?.account_id || `legacy_account_${idx}`,
    _index: idx,
  }));
  const rows = parseJSON(record.account_rows_json);
  const accountMap = new Map(accounts.map((acc) => [acc.account_id, acc]));
  const groups = [];
  const groupMap = new Map();

  rows.forEach((row) => {
    const hasMatch = row?.account_id && accountMap.has(row.account_id);
    const groupKey = hasMatch ? row.account_id : "__unlinked__";
    if (!groupMap.has(groupKey)) {
      if (hasMatch) {
        const acc = accountMap.get(row.account_id);
        groupMap.set(groupKey, {
          label: `账户${(acc?._index ?? 0) + 1}: ${acc?.account_name || "未填写开户银行"}${acc?.account_no ? ` - ${acc.account_no}` : ""}`,
          rows: [],
        });
      } else {
        groupMap.set(groupKey, {
          label: "未关联账户（历史数据）",
          rows: [],
        });
      }
      groups.push(groupMap.get(groupKey));
    }
    groupMap.get(groupKey).rows.push(row);
  });

  return groups;
};

const getAccountYearlyTotals = () => {
  const record = tableData.value[0] || {};
  return calculateAccountYearlyTotals(parseJSON(record.account_rows_json));
};

async function fetchTableDataWithSearch() {
  try {
    const response = await axios.get(
      `${API_NODE}/loan-application?projectNumber=${encodeURIComponent(searchName.value)}`
    );
    const response2 = await axios.get(
      `${API_NODE}/datahuizong?name=${encodeURIComponent(searchName.value)}`
    );
    tableData.value = response.data || [];
    tableData2.value = response2.data || [];
  } catch (error) {
    console.error("Error fetching table data:", error);
  }
}

onMounted(() => {
  const reportNumber = route.params.reportNumber;
  if (reportNumber) {
    searchName.value = reportNumber;
    fetchTableDataWithSearch();
  }
});

onActivated(() => {
  const reportNumber = route.params.reportNumber;
  if (reportNumber) {
    searchName.value = reportNumber;
    fetchTableDataWithSearch();
  }
});
</script>

<style scoped>
.detail-page {
  max-width: 1400px;
  margin: 0 auto;
  padding: 16px;
  background: #f5f7fa;
}

.section-card {
  background: #fff;
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
  margin-bottom: 16px;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 16px;
}

.section-header h3 {
  margin: 0 0 8px 0;
  color: #235f68;
  font-size: 16px;
  font-weight: 600;
  border-bottom: 2px solid #2b7a78;
  padding-bottom: 8px;
}

.subtitle {
  margin: 0;
  color: #999;
  font-size: 12px;
}

.info-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 12px;
}

.info-item {
  display: flex;
  flex-direction: column;
  gap: 6px;
  background: #fafafa;
  border: 1px solid #f0f0f0;
  border-radius: 6px;
  padding: 10px;
}

.info-item.full-width {
  grid-column: 1 / -1;
}

.info-item label {
  color: #888;
  font-size: 12px;
}

.info-item span {
  color: #1f1f1f;
  font-size: 14px;
  word-break: break-all;
}

.info-item span.highlight {
  color: #2b7a78;
  font-weight: 600;
  font-size: 16px;
}

.value-text {
  color: #1f1f1f;
  font-size: 14px;
  word-break: break-all;
  white-space: pre-wrap;
  line-height: 1.5;
  max-height: 150px;
  overflow-y: auto;
}

.text-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 12px;
}

.text-block {
  background: #fafafa;
  border-radius: 6px;
  padding: 12px;
  border: 1px solid #f0f0f0;
}

.text-block h4 {
  margin: 0 0 8px 0;
  color: #235f68;
  font-size: 14px;
  font-weight: 600;
}

.text-box {
  min-height: 100px;
  max-height: 200px;
  overflow-y: auto;
  padding: 8px;
  background: #fff;
  border: 1px solid #eee;
  border-radius: 4px;
  white-space: pre-wrap;
  line-height: 1.5;
  font-size: 13px;
  color: #333;
}

.predict-box {
  background: #f7f9fb;
  border-radius: 8px;
  padding: 12px;
  min-height: 120px;
  max-height: 300px;
  overflow-y: auto;
  border: 1px solid #e8e8e8;
  line-height: 1.6;
  font-size: 13px;
  color: #333;
  margin-bottom: 16px;
}

.expert-opinion {
  background: #fafafa;
  border-radius: 6px;
  padding: 12px;
  margin-top: 12px;
  border: 1px solid #f0f0f0;
}

.expert-opinion .label {
  color: #888;
  font-size: 12px;
  margin-bottom: 6px;
}

.expert-opinion .value-text {
  margin: 0;
}

.sub-table {
  margin-top: 12px;
  padding: 12px;
  background: #f7f9fa;
  border-radius: 6px;
  border: 1px dashed #d9d3c8;
}

.sub-table-title {
  font-weight: 600;
  color: #235f68;
  margin-bottom: 8px;
  font-size: 13px;
}

.sub-item {
  padding: 8px 0;
  border-bottom: 1px solid #e6e6e6;
  font-size: 13px;
  color: #333;
}

.sub-item:last-child {
  border-bottom: none;
}

.sub-item-content {
  margin-top: 6px;
  margin-left: 16px;
  font-size: 12px;
  color: #666;
}

.sub-item-content div {
  padding: 2px 0;
}

.th-bold {
  font-weight: 700;
}

.loading {
  padding: 40px;
  text-align: center;
  color: #999;
}
</style>
