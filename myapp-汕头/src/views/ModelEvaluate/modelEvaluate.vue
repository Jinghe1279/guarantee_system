<template>
  <div class="page">
    <div class="page-header">
      <div>
        <h2>模型评估</h2>
        <p class="subtitle">对我提交的记录重新调用模型预测并保存结果</p>
      </div>
      <div class="actions">
        <span class="count">共 {{ records.length }} 条</span>
        <a-button type="link" @click="fetchEntries" :loading="loading">刷新</a-button>
      </div>
    </div>

    <a-alert
      v-if="!username"
      message="请先登录后再查看录入记录"
      type="warning"
      show-icon
      style="margin-bottom: 16px;"
    />

    <a-table
      v-else
      :columns="columns"
      :data-source="records"
      :loading="loading"
      :row-key="row => row.id || row.project_number"
      bordered
    >
      <template #bodyCell="{ column, record }">
        <template v-if="column.key === 'model_opinion'">
          <a-button type="link" @click="showModelOpinion(record)">查看意见</a-button>
        </template>
        <template v-else-if="column.key === 'actions'">
          <a-space>
            <a-button type="link" :loading="predictingId === (record.id || record.project_number)" @click="handlePredict(record)">预测</a-button>
            <a-button type="link" @click="downloadRecord(record)">下载</a-button>
            <a-popconfirm
              title="确认删除这条记录吗？"
              ok-text="删除"
              cancel-text="取消"
              @confirm="() => deleteRecord(record)"
            >
              <a-button type="link" danger>删除</a-button>
            </a-popconfirm>
          </a-space>
        </template>
      </template>

      <template #expandedRowRender="{ record }">
        <div class="detail-container">
          <!-- 项目信息 -->
          <div class="detail-section">
            <h4 class="section-title">项目信息</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">企业编号</div>
                <div class="value">{{ formatValue(record.project_enterpriseid) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">企业名称</div>
                <div class="value">{{ formatValue(record.company_name) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">项目经理</div>
                <div class="value">{{ formatValue(record.project_market_manager) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">项目A角</div>
                <div class="value">{{ formatValue(record.project_a_owner) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">项目B角</div>
                <div class="value">{{ formatValue(record.project_b_owner) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">市场经理</div>
                <div class="value">{{ formatValue(record.project_market_manager) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">项目来源</div>
                <div class="value">{{ formatValue(record.project_source) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">合作银行</div>
                <div class="value">{{ formatValue(record.project_coop_bank) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">申请日期</div>
                <div class="value">{{ formatDateTime(record.project_apply_date) }}</div>
              </div>
            </div>
          </div>

          <!-- 申请贷款信息 -->
          <div class="detail-section">
            <h4 class="section-title">申请贷款信息</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">借款人名称</div>
                <div class="value">{{ formatValue(record.loan_borrower_name) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">申请金额（万元）</div>
                <div class="value">{{ formatValue(record.loan_apply_amount) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">申请期限（月）</div>
                <div class="value">{{ formatValue(record.loan_apply_term) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">还款方式</div>
                <div class="value">{{ formatValue(record.analysis_plan_repayment_method) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">贷款用途</div>
                <div class="value">{{ formatValue(record.loan_purpose_detail) }}</div>
              </div>
            </div>
          </div>

          <!-- 企业基本情况 -->
          <div class="detail-section">
            <h4 class="section-title">企业基本情况</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">注册资本（万元）</div>
                <div class="value">{{ formatValue(record.company_registered_capital) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">成立时间</div>
                <div class="value">{{ formatDateTime(record.company_established_date) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">注册地址</div>
                <div class="value">{{ formatValue(record.company_registered_address) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">所属行业</div>
                <div class="value">{{ formatValue(record.business_type) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">员工人数</div>
                <div class="value">{{ formatValue(record.company_employee_count) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">是否外贸型</div>
                <div class="value">{{ formatValue(record.business_is_waimao) || formatDisplayValue('is_foreign_trade', record.is_foreign_trade) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">是否慎入行业</div>
                <div class="value">{{ formatValue(record.business_is_jinshen) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">最近工资是否发放</div>
                <div class="value">{{ formatValue(record.company_is_salary) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">主营业务</div>
                <div class="value">{{ formatValue(record.company_main_business) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">股东情况</div>
                <div class="value">{{ formatValue(record.company_shareholder_info) }}</div>
              </div>
            </div>
          </div>

          <!-- 实控人信息 -->
          <div class="detail-section">
            <h4 class="section-title">实控人/申请人情况</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">实际控制人</div>
                <div class="value">{{ formatValue(record.controller_name) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">性别</div>
                <div class="value">{{ formatValue(record.controller_gender) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">籍贯</div>
                <div class="value">{{ formatValue(record.controller_native_place) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">婚姻状况</div>
                <div class="value">{{ formatValue(record.controller_marital_status) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">出生日期</div>
                <div class="value">{{ formatDateTime(record.controller_birth_date) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">文化程度</div>
                <div class="value">{{ formatValue(record.controller_education) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">从业年限</div>
                <div class="value">{{ formatValue(record.controller_service_years) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">配偶姓名</div>
                <div class="value">{{ formatValue(record.controller_spouse_name) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">学习、工作及企业发展经历</div>
                <div class="value">{{ formatValue(record.controller_career_experience) }}</div>
              </div>
            </div>
          </div>

          <!-- 家庭与社会情况 -->
          <div class="detail-section">
            <h4 class="section-title">家庭与社会情况</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">家庭是否和睦</div>
                <div class="value">{{ formatValue(record.family_is_hemu) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">家庭年支出</div>
                <div class="value">{{ formatValue(record.family_annual_expense) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">家庭成员情况</div>
                <div class="value">{{ formatValue(record.family_members_info) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">社会关系</div>
                <div class="value">{{ formatValue(record.social_relationship_info) }}</div>
              </div>
            </div>
          </div>

          <!-- 居住情况 -->
          <div class="detail-section">
            <h4 class="section-title">居住情况</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">居住场所类型</div>
                <div class="value">{{ formatValue(record.residence_type) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">本地居住年限（年）</div>
                <div class="value">{{ formatValue(record.residence_years) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">居住地址</div>
                <div class="value">{{ formatValue(record.residence_address) }}</div>
              </div>
            </div>
          </div>

          <!-- 经营场所 -->
          <div class="detail-section">
            <h4 class="section-title">经营场所</h4>
            <div class="detail-grid">
              <div class="detail-item full-width">
                <div class="label">商业模式简述</div>
                <div class="value">{{ formatValue(record.business_model_description) }}</div>
              </div>
            </div>
            <div v-if="record.business_sites_json" class="sub-table">
              <div class="sub-table-title">经营场所明细</div>
              <div v-for="(site, idx) in parseJSON(record.business_sites_json)" :key="idx" class="sub-item">
                <span><strong>场所{{ idx + 1 }}:</strong> {{ site.address }} | 建筑面积（㎡）: {{ site.building_area }} | 土地面积（㎡）: {{ site.land_area }} | {{ site.ownership }} | 场地月租金（万元）: {{ site.month_pay }} | 最近一期是否支付: {{ site.is_pay || '—' }}</span>
              </div>
            </div>
          </div>

          <!-- 资信状况 -->
          <div class="detail-section">
            <h4 class="section-title">资信状况</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">征信查询次数</div>
                <div class="value">{{ formatValue(record.credit_inquiry_count) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">逾期次数（次）</div>
                <div class="value">{{ formatValue(record.credit_overdue_count) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">最大逾期金额（万元）</div>
                <div class="value">{{ formatValue(record.credit_max_overdue_amount) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">征信不良信息</div>
                <div class="value">{{ formatValue(record.credit_adverse_info) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">诉讼情况</div>
                <div class="value">{{ formatValue(record.litigation_status) }}</div>
              </div>
            </div>
            <div v-if="record.business_accounts_json" class="sub-table">
              <div class="sub-table-title">银行账户</div>
              <div v-for="(acc, idx) in parseJSON(record.business_accounts_json)" :key="idx" class="sub-item">
                <span><strong>账户{{ idx + 1 }}:</strong> {{ acc.account_name }} - {{ acc.account_no }}</span>
              </div>
            </div>
            <div v-if="getAccountBalanceGroups(record).length" class="sub-table">
              <div class="sub-table-title">银行月末余额</div>
              <div v-for="(group, gIdx) in getAccountBalanceGroups(record)" :key="`acc-group-${gIdx}`" class="sub-item">
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
            <div v-if="getAccountYearlyTotals(record).length" class="sub-table">
              <div class="sub-table-title">月末余额合计（按年份）</div>
              <div v-for="(row, idx) in getAccountYearlyTotals(record)" :key="`acc-total-${idx}`" class="sub-item">
                <div><strong>{{ row.year }}年:</strong></div>
                <div style="margin-left: 20px; margin-top: 4px;">
                  1月余额合计（万元）: {{ row.m1 }} | 2月余额合计（万元）: {{ row.m2 }} | 3月余额合计（万元）: {{ row.m3 }} | 4月余额合计（万元）: {{ row.m4 }} | 5月余额合计（万元）: {{ row.m5 }} | 6月余额合计（万元）: {{ row.m6 }} |
                  7月余额合计（万元）: {{ row.m7 }} | 8月余额合计（万元）: {{ row.m8 }} | 9月余额合计（万元）: {{ row.m9 }} | 10月余额合计（万元）: {{ row.m10 }} | 11月余额合计（万元）: {{ row.m11 }} | 12月余额合计（万元）: {{ row.m12 }} |
                  <strong>月均余额合计（万元）: {{ row.avg }}</strong>
                </div>
              </div>
            </div>
            <div v-if="record.daily_avg_balance_json" class="sub-table">
              <div class="sub-table-title">日均余额（万元）</div>
              <div v-for="(row, idx) in parseJSON(record.daily_avg_balance_json)" :key="idx" class="sub-item">
                <div><strong>{{ row.year }}年:</strong></div>
                <div style="margin-left: 20px; margin-top: 4px;">
                  3月日均（万元）: {{ row.m3 }} | 6月日均（万元）: {{ row.m6 }} | 9月日均（万元）: {{ row.m9 }} | 12月日均（万元）: {{ row.m12 }} |
                  <strong>全年日均（万元）: {{ row.annual_avg }}</strong>
                </div>
              </div>
            </div>
          </div>

          <!-- 财务数据 -->
          <div class="detail-section">
            <h4 class="section-title">财务数据</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">报表日期</div>
                <div class="value">{{ formatDateTime(record.bs_date) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">货币资金（万元）</div>
                <div class="value">{{ formatValue(record.bs_cash) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">应收账款（万元）</div>
                <div class="value">{{ formatValue(record.bs_ar) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">存货（万元）</div>
                <div class="value">{{ formatValue(record.bs_inventory) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">应付账款（万元）</div>
                <div class="value">{{ formatValue(record.bs_ap) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">总资产（万元）</div>
                <div class="value">{{ formatValue(record.bs_total_assets || record.analysis_fin_total_assets) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">总负债（万元）</div>
                <div class="value">{{ formatValue(record.analysis_fin_total_liabilities) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">净资产（万元）</div>
                <div class="value">{{ formatValue(record.analysis_fin_net_assets) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">营业收入（万元）</div>
                <div class="value">{{ formatValue(record.analysis_fin_revenue || record.is_table_s_total) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">净利润（万元）</div>
                <div class="value">{{ formatValue(record.is_table_net_profit || record.analysis_fin_net_income) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">年净收益（万元）</div>
                <div class="value">{{ formatValue(record.is_table_annual_net_income) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">利润用途</div>
                <div class="value">{{ formatValue(record.analysis_profit_destination) }}</div>
              </div>
            </div>
            <div v-if="record.is_table_sales_list_json" class="sub-table">
              <div class="sub-table-title">销售收入明细</div>
              <div v-for="(item, idx) in parseJSON(record.is_table_sales_list_json)" :key="idx" class="sub-item">
                <span><strong>{{ item.name }}:</strong> {{ item.value }}万元</span>
              </div>
            </div>
          </div>

          <!-- 指标评价 -->
          <div class="detail-section">
            <h4 class="section-title">指标评价</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">资产负债率（%）</div>
                <div class="value">{{ formatValue(record.analysis_ind_asset_debt_ratio) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">销售负债率（%）</div>
                <div class="value">{{ formatValue(record.analysis_ind_sales_debt_ratio) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">是否符合净收益3倍</div>
                <div class="value">{{ formatValue(record.analysis_ind_meets_3x_income) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">应收账款平均账期（天）</div>
                <div class="value">{{ formatValue(record.analysis_ind_receivable_days) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">月均/日均余额（万元）</div>
                <div class="value">{{ formatValue(record.analysis_ind_avg_balance) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">月还款/月净收益（%）</div>
                <div class="value">{{ formatValue(record.analysis_ind_repayment_ratio) }}</div>
              </div>
            </div>
          </div>

          <!-- 担保措施 -->
          <div class="detail-section">
            <h4 class="section-title">担保措施</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">贷款金额（万元）</div>
                <div class="value">{{ formatValue(record.analysis_plan_amount) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">贷款期限（月）</div>
                <div class="value">{{ formatValue(record.analysis_plan_term) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">担保费率（%）</div>
                <div class="value">{{ formatValue(record.analysis_plan_fee_rate) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">企业保证</div>
                <div class="value">{{ formatValue(record.analysis_plan_corp_guarantee) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">个人保证</div>
                <div class="value">{{ formatValue(record.analysis_plan_personal_guarantee) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">是否有额外担保人</div>
                <div class="value">{{ formatValue(record.analysis_ind_is_added_guarantor) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">抵押物</div>
                <div class="value">{{ formatValue(record.analysis_plan_collateral) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">房产抵押评估值（万元）</div>
                <div class="value">{{ formatValue(record.analysis_plan_diyapingguzhi) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">房产二押余值（万元）</div>
                <div class="value">{{ formatValue(record.analysis_plan_eryayuzhi) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">设备抵押净值（万元）</div>
                <div class="value">{{ formatValue(record.analysis_plan_diyajingzhi) }}</div>
              </div>
            </div>
            <div v-if="record.guarantees_json" class="sub-table">
              <div class="sub-table-title">我司在保情况</div>
              <div v-for="(g, idx) in parseJSON(record.guarantees_json)" :key="idx" class="sub-item">
                <span><strong>在保{{ idx + 1 }}:</strong> {{ g.type }} | 承保金额（万元）: {{ g.amount }} | 承保余额（万元）: {{ g.balance }} | 月还款本息（万元）: {{ g.monthly_payment }}</span>
              </div>
              <div v-if="record.guarantees_amount_total || record.guarantees_balance_total" class="sub-item" style="font-weight: 600; border-top: 2px solid #2b7a78; padding-top: 8px; margin-top: 8px;">
                <span>合计 - 承保金额（万元）: {{ formatValue(record.guarantees_amount_total) }} | 承保余额（万元）: {{ formatValue(record.guarantees_balance_total) }}</span>
              </div>
            </div>
            <div v-if="getExistingLoanSubjectGroups(record).length" class="sub-table">
              <div class="sub-table-title">现有贷款情况</div>
              <div v-for="(subject, sIdx) in getExistingLoanSubjectGroups(record)" :key="`loan-subject-${sIdx}`" class="sub-item">
                <div><strong>贷款主体{{ sIdx + 1 }}: {{ subject.subject_name }}</strong></div>
                <div v-for="(loan, idx) in subject.loans" :key="`loan-${sIdx}-${idx}`" style="margin-left: 12px; margin-top: 4px;">
                  <span>贷款{{ idx + 1 }}: {{ loan.type }} | 贷款金额（万元）: {{ loan.amount }} | 贷款余额（万元）: {{ loan.balance }} | 月还款本息（万元）: {{ loan.monthly_payment }}</span>
                </div>
              </div>
              <div
                v-if="record.existing_loans_amount_total || record.existing_loans_balance_total || record.existing_loans_monthly_payment_total"
                class="sub-item"
                style="font-weight: 600; border-top: 2px solid #2b7a78; padding-top: 8px; margin-top: 8px;"
              >
                <span>
                  合计 - 贷款金额（万元）: {{ formatValue(record.existing_loans_amount_total) }} |
                  贷款余额（万元）: {{ formatValue(record.existing_loans_balance_total) }} |
                  月还款本息（万元）: {{ formatValue(record.existing_loans_monthly_payment_total) }}
                </span>
              </div>
            </div>
          </div>

          <!-- 其他信息 -->
          <div class="detail-section">
            <h4 class="section-title">其他信息</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">是否成长阶段</div>
                <div class="value">{{ formatValue(record.analysis_ind_is_growth_phase) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">是否使用友贷宝</div>
                <div class="value">{{ formatValue(record.analysis_ind_is_superior_loan) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">用电情况</div>
                <div class="value">{{ formatValue(record.electricity_is_quantity === '是' ? '采集用电量' : record.electricity_is_cost === '是' ? '采集电费' : '—') }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">用电情况说明</div>
                <div class="value">{{ formatValue(record.electricity_descript) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">软信息分析</div>
                <div class="value">{{ formatValue(record.analysis_soft_info) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">综合评价</div>
                <div class="value">{{ formatValue(record.analysis_summary) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">额度测定</div>
                <div class="value">{{ formatValue(record.analysis_limit_calculation) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">增额因素</div>
                <div class="value">{{ formatValue(record.analysis_limit_increase_factors) }}</div>
              </div>
            </div>
            <div v-if="record.electricity_items_json" class="sub-table">
              <div class="sub-table-title">用电明细</div>
              <div v-for="(item, idx) in parseJSON(record.electricity_items_json)" :key="idx" class="sub-item">
                <div><strong>{{ item.year }}年:</strong></div>
                <div style="margin-left: 20px; margin-top: 4px;">
                  1月: {{ item.m1 }} | 2月: {{ item.m2 }} | 3月: {{ item.m3 }} | 4月: {{ item.m4 }} | 5月: {{ item.m5 }} | 6月: {{ item.m6 }} |
                  7月: {{ item.m7 }} | 8月: {{ item.m8 }} | 9月: {{ item.m9 }} | 10月: {{ item.m10 }} | 11月: {{ item.m11 }} | 12月: {{ item.m12 }} |
                  <strong>合计: {{ item.total }}{{ record.electricity_is_quantity === '是' ? 'kWh' : '万元' }}</strong>
                </div>
              </div>
            </div>
            <div v-if="record.asset_stats_json" class="sub-table">
              <div class="sub-table-title">资产统计</div>
              <div v-for="(asset, idx) in parseJSON(record.asset_stats_json)" :key="idx" class="sub-item">
                <span><strong>{{ asset.name }}:</strong> 购入价格（万元） {{ asset.buy_price }} | 当前价值（万元） {{ asset.current_value }} | 折旧（万元） {{ asset.depreciation }}</span>
              </div>
              <div v-if="record.asset_totals_buy_price || record.asset_totals_current_value" class="sub-item" style="font-weight: 600; border-top: 2px solid #2b7a78; padding-top: 8px; margin-top: 8px;">
                <span>合计 - 购入价格（万元）: {{ formatValue(record.asset_totals_buy_price) }} | 当前价值（万元）: {{ formatValue(record.asset_totals_current_value) }} | 折旧（万元）: {{ formatValue(record.asset_totals_depreciation) }}</span>
              </div>
            </div>
            <div v-if="record.cashflow_in_json" class="sub-table">
              <div class="sub-table-title">银行流水流入（万元）</div>
              <div v-for="(item, idx) in parseJSON(record.cashflow_in_json)" :key="idx" class="sub-item">
                <div><strong>{{ item.year }}年:</strong></div>
                <div style="margin-left: 20px; margin-top: 4px;">
                  1月金额（万元）: {{ item.m1 }} | 2月金额（万元）: {{ item.m2 }} | 3月金额（万元）: {{ item.m3 }} | 4月金额（万元）: {{ item.m4 }} | 5月金额（万元）: {{ item.m5 }} | 6月金额（万元）: {{ item.m6 }} |
                  7月金额（万元）: {{ item.m7 }} | 8月金额（万元）: {{ item.m8 }} | 9月金额（万元）: {{ item.m9 }} | 10月金额（万元）: {{ item.m10 }} | 11月金额（万元）: {{ item.m11 }} | 12月金额（万元）: {{ item.m12 }} |
                  <strong>合计（万元）: {{ item.total }}</strong>
                </div>
              </div>
            </div>
            <div v-if="record.cashflow_out_json" class="sub-table">
              <div class="sub-table-title">银行流水流出（万元）</div>
              <div v-for="(item, idx) in parseJSON(record.cashflow_out_json)" :key="idx" class="sub-item">
                <div><strong>{{ item.year }}年:</strong></div>
                <div style="margin-left: 20px; margin-top: 4px;">
                  1月金额（万元）: {{ item.m1 }} | 2月金额（万元）: {{ item.m2 }} | 3月金额（万元）: {{ item.m3 }} | 4月金额（万元）: {{ item.m4 }} | 5月金额（万元）: {{ item.m5 }} | 6月金额（万元）: {{ item.m6 }} |
                  7月金额（万元）: {{ item.m7 }} | 8月金额（万元）: {{ item.m8 }} | 9月金额（万元）: {{ item.m9 }} | 10月金额（万元）: {{ item.m10 }} | 11月金额（万元）: {{ item.m11 }} | 12月金额（万元）: {{ item.m12 }} |
                  <strong>合计（万元）: {{ item.total }}</strong>
                </div>
              </div>
            </div>
          </div>

          <!-- 预测结果 -->
          <div class="detail-section">
            <h4 class="section-title">预测结果</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">预测额度(万元)</div>
                <div class="value highlight">{{ formatValue(record.predicted) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">专家测算额度(万元)</div>
                <div class="value highlight">{{ formatValue(record.expert_amount) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">预测结果文本</div>
                <div class="value">{{ formatValue(record.prediction_text) }}</div>
              </div>
              <div class="detail-item full-width">
                <div class="label">专家意见</div>
                <div class="value">{{ formatValue(record.expert_opinion) }}</div>
              </div>
            </div>
          </div>

          <!-- 创建信息 -->
          <div class="detail-section">
            <h4 class="section-title">创建信息</h4>
            <div class="detail-grid">
              <div class="detail-item">
                <div class="label">创建人</div>
                <div class="value">{{ formatValue(record.created_by) }}</div>
              </div>
              <div class="detail-item">
                <div class="label">创建时间</div>
                <div class="value">{{ formatDateTime(record.created_at) }}</div>
              </div>
            </div>
          </div>
        </div>
      </template>
    </a-table>

    <a-modal
      v-model:visible="modelOpinionVisible"
      title="模型意见"
      :width="760"
      :mask-closable="false"
      :ok-button-props="{ style: { display: 'none' } }"
      cancel-text="确定"
      @cancel="modelOpinionVisible = false"
    >
      <div class="model-opinion-content">{{ modelOpinionText || '暂无模型意见' }}</div>
    </a-modal>
  </div>
</template>

<script lang="ts" setup>
import { computed, onMounted, ref, h } from 'vue';
import axios from 'axios';
import { message, Modal } from 'ant-design-vue';
import { API_NODE, API_FLASK } from "@/api/config";

interface RecordItem {
  [key: string]: any;
}

const username = ref<string>(localStorage.getItem('username') || '');
const records = ref<RecordItem[]>([]);
const loading = ref<boolean>(false);
const predictingId = ref<string | number | null>(null);
const modelOpinionVisible = ref<boolean>(false);
const modelOpinionText = ref<string>('');
const predictionModalVisible = ref<boolean>(false);
const predictionResult = ref<{ amount?: number; report?: string }>({});
const businessSites = ref<RecordItem[]>([]);
const businessAccounts = ref<RecordItem[]>([]);
const accountRows = ref<RecordItem[]>([]);
const dailyAvgBalance = ref<RecordItem[]>([]);
const guarantees = ref<RecordItem[]>([]);
const existingLoans = ref<RecordItem[]>([]);
const electricityItems = ref<RecordItem[]>([]);
const assetStats = ref<RecordItem[]>([]);
const revCheckItems = ref<RecordItem[]>([]);
const cashflowIn = ref<RecordItem[]>([]);
const cashflowOut = ref<RecordItem[]>([]);
const salesList = ref<RecordItem[]>([]);

const monthFields = Array.from({ length: 12 }, (_, index) => ({
  key: `m${index + 1}`,
  label: `${index + 1}月`,
}));
const quarterFields = ['m3', 'm6', 'm9', 'm12'].map((key) => ({
  key,
  label: `${key.slice(1)}月`,
}));

const parseArrayField = (value: any) => {
  if (!value) return [];
  if (Array.isArray(value)) return value;
  if (typeof value === 'string') {
    try {
      const parsed = JSON.parse(value);
      return Array.isArray(parsed) ? parsed : [];
    } catch {
      return [];
    }
  }
  return [];
};

const ensureArray = (items: any[], factory: () => RecordItem) =>
  items.length > 0 ? items : [factory()];

const createBusinessSite = () => ({
  address: '',
  building_area: '',
  land_area: '',
  ownership: '',
  month_pay: '',
  is_pay: '',
});

const createBusinessAccount = () => ({
  account_name: '',
  account_no: '',
});

const createAccountRow = () =>
  monthFields.reduce(
    (acc, month) => ({ ...acc, [month.key]: '' }),
    { year: '', avg: '' }
  );

const createDailyAvgRow = () =>
  quarterFields.reduce(
    (acc, month) => ({ ...acc, [month.key]: '' }),
    { year: '', annual_avg: '' }
  );

const createGuarantee = () => ({
  type: '',
  amount: '',
  balance: '',
  counter_guarantee: '',
  monthly_payment: '',
  start_date: '',
  end_date: '',
  bank_rate: '',
  purpose: '',
});

const createExistingLoan = () => ({
  type: '',
  amount: '',
  balance: '',
  mode: '',
  monthly_payment: '',
  start_date: '',
  end_date: '',
  bank_rate: '',
  purpose: '',
});

const createElectricityItem = () =>
  monthFields.reduce(
    (acc, month) => ({ ...acc, [month.key]: '' }),
    { year: '', total: '' }
  );

const createAssetStat = () => ({
  name: '',
  buy_time: '',
  buy_price: '',
  current_value: '',
  depreciation: '',
  remark: '',
});

const createRevCheckItem = () => ({
  name: '',
  value: '',
});

const createCashflowItem = () =>
  monthFields.reduce(
    (acc, month) => ({ ...acc, [month.key]: '' }),
    { year: '', total: '' }
  );

const createSalesItem = () => ({
  name: '',
  value: '',
});

const addBusinessSite = () => businessSites.value.push(createBusinessSite());
const removeBusinessSite = (index: number) => businessSites.value.splice(index, 1);
const addBusinessAccount = () => businessAccounts.value.push(createBusinessAccount());
const removeBusinessAccount = (index: number) => businessAccounts.value.splice(index, 1);
const addAccountRow = () => accountRows.value.push(createAccountRow());
const removeAccountRow = (index: number) => accountRows.value.splice(index, 1);
const addDailyAvgRow = () => dailyAvgBalance.value.push(createDailyAvgRow());
const removeDailyAvgRow = (index: number) => dailyAvgBalance.value.splice(index, 1);
const addGuarantee = () => guarantees.value.push(createGuarantee());
const removeGuarantee = (index: number) => guarantees.value.splice(index, 1);
const addExistingLoan = () => existingLoans.value.push(createExistingLoan());
const removeExistingLoan = (index: number) => existingLoans.value.splice(index, 1);
const addElectricityItem = () => electricityItems.value.push(createElectricityItem());
const removeElectricityItem = (index: number) => electricityItems.value.splice(index, 1);
const addAssetStat = () => assetStats.value.push(createAssetStat());
const removeAssetStat = (index: number) => assetStats.value.splice(index, 1);
const addRevCheckItem = () => revCheckItems.value.push(createRevCheckItem());
const removeRevCheckItem = (index: number) => revCheckItems.value.splice(index, 1);
const addCashflowIn = () => cashflowIn.value.push(createCashflowItem());
const removeCashflowIn = (index: number) => cashflowIn.value.splice(index, 1);
const addCashflowOut = () => cashflowOut.value.push(createCashflowItem());
const removeCashflowOut = (index: number) => cashflowOut.value.splice(index, 1);
const addSalesItem = () => salesList.value.push(createSalesItem());
const removeSalesItem = (index: number) => salesList.value.splice(index, 1);

const columns = [
  { title: '企业编号', dataIndex: 'project_number', key: 'project_number' },
  { title: '企业名称', dataIndex: 'company_name', key: 'company_name' },
  { title: '申请金额（万元）', dataIndex: 'application_amount', key: 'application_amount' },
  { title: '申请期限（月）', dataIndex: 'application_period', key: 'application_period' },
  { title: '预测额度(万元)', dataIndex: 'predicted', key: 'predicted' },
  { title: '模型意见', key: 'model_opinion' },
  { title: '专家额度', dataIndex: 'expert_amount', key: 'expert_amount' },
  { title: '创建时间', dataIndex: 'created_at', key: 'created_at', customRender: ({ text }: any) => formatDateTime(text) },
  { title: '操作', key: 'actions' },
];

const fieldLabels: Record<string, string> = {
  id: 'ID',
  project_number: '企业编号',
  company_name: '企业名称',
  project_manager: '项目经理',
  application_amount: '申请金额（万元）',
  application_period: '申请期限（月）',
  repayment_method: '还款方式',
  controller_gender: '实控人性别',
  education_level: '实控人文化程度',
  marital_status: '婚姻状况',
  residence_type: '居住场所类型',
  local_residence_years: '本地居住年限（年）',
  industry_category: '所属行业(大类)',
  industry_experience: '借款人从业年限',
  is_foreign_trade: '是否外贸型',
  is_cautious_industry: '是否慎入行业',
  employee_count: '企业雇佣人数',
  business_premises_type: '经营场所类型',
  monthly_rent: '场地月租金（万元）',
  monthly_balance: '月均余额（万元）',
  daily_balance: '日均余额（万元）',
  electricity_consumption: '用电量/电费（kWh/万元）',
  cash_at_meeting: '上会时点货币资金（万元）',
  receivables_at_meeting: '上会时点应收账款（万元）',
  inventory_at_meeting: '上会时点存货（万元）',
  payables_at_meeting: '上会时点应付账款（万元）',
  total_assets: '总资产（万元）',
  total_liabilities: '总负债（万元）',
  net_assets: '净资产（万元）',
  annual_sales: '年销售收入（万元）',
  annual_net_profit: '年净利润（万元）',
  monthly_net_profit: '月净利润（万元）',
  core_assets: '核心资产（万元）',
  hard_liabilities: '硬性负债（万元）',
  operating_liabilities: '经营负债（万元）',
  sales_debt_ratio: '销售/负债比（%）',
  asset_debt_ratio: '资产/负债比（%）',
  monthly_repayment: '月还款额（万元）',
  total_monthly_repayment: '总月还款额（万元）',
  repayment_income_ratio: '月还款/月净利润（%）',
  average_payment_period: '平均付款账期（天）',
  family_harmony: '家庭和睦度',
  minor_children: '未成年子女数',
  adult_family_members: '成年家庭成员数',
  working_family_members: '在职家庭成员数',
  credit_inquiries: '征信查询次数',
  overdue_times: '逾期次数（次）',
  max_overdue_amount: '最大逾期金额（万元）',
  bank_inflow: '银行流水流入（万元）',
  bank_outflow: '银行流水流出（万元）',
  highest_flow_month: '流水峰值月份',
  lowest_flow_month: '流水低谷月份',
  company_guarantee: '企业保证',
  personal_guarantee: '个人保证',
  additional_guarantor: '是否有额外担保人',
  property_mortgage: '房产抵押',
  property_second_mortgage: '房产二抵',
  equipment_mortgage: '设备抵押',
  is_growth_stage: '是否成长阶段',
  used_youdaibao: '是否使用友贷宝',
  education_work_experience: '教育/工作经历',
  family_social_relations: '家庭成员和社会关系',
  business_model: '商业模式',
  counter_guarantee: '反担保措施',
  main_business: '主营业务',
  profit_usage: '利润用途',
  other_soft_info: '其他软信息',
  loan_purpose: '贷款用途',
  predicted: '预测额度（万元）',
  prediction_text: '预测结果文本',
  expert_opinion: '专家意见',
  expert_amount: '专家测算额度（万元）',
  created_by: '创建人',
  created_at: '创建时间',
};

const detailOrder = [
  'project_number',
  'company_name',
  'project_manager',
  'application_amount',
  'application_period',
  'repayment_method',
  'controller_gender',
  'education_level',
  'marital_status',
  'residence_type',
  'local_residence_years',
  'industry_category',
  'industry_experience',
  'is_foreign_trade',
  'is_cautious_industry',
  'employee_count',
  'business_premises_type',
  'monthly_rent',
  'monthly_balance',
  'daily_balance',
  'electricity_consumption',
  'cash_at_meeting',
  'receivables_at_meeting',
  'inventory_at_meeting',
  'payables_at_meeting',
  'total_assets',
  'total_liabilities',
  'net_assets',
  'annual_sales',
  'annual_net_profit',
  'monthly_net_profit',
  'core_assets',
  'hard_liabilities',
  'operating_liabilities',
  'sales_debt_ratio',
  'asset_debt_ratio',
  'monthly_repayment',
  'total_monthly_repayment',
  'repayment_income_ratio',
  'average_payment_period',
  'family_harmony',
  'minor_children',
  'adult_family_members',
  'working_family_members',
  'credit_inquiries',
  'overdue_times',
  'max_overdue_amount',
  'bank_inflow',
  'bank_outflow',
  'highest_flow_month',
  'lowest_flow_month',
  'company_guarantee',
  'personal_guarantee',
  'additional_guarantor',
  'property_mortgage',
  'property_second_mortgage',
  'equipment_mortgage',
  'is_growth_stage',
  'used_youdaibao',
  'education_work_experience',
  'family_social_relations',
  'business_model',
  'counter_guarantee',
  'main_business',
  'profit_usage',
  'other_soft_info',
  'loan_purpose',
  'predicted',
  'prediction_text',
  'expert_opinion',
  'expert_amount',
  'created_by',
  'created_at',
];

const getDisplayEntries = (record: RecordItem) => {
  const extraKeys = Object.keys(record).filter((key) => !detailOrder.includes(key));
  const orderedKeys = [...detailOrder, ...extraKeys];

  return orderedKeys.map((key) => ({
    key,
    label: fieldLabels[key] || key,
    value: formatDisplayValue(key, record[key]),
  }));
};

const formatValue = (value: any) => {
  if (value === null || value === undefined || value === '') return '—';
  return value;
};

const formatDateTime = (val: any) => {
  if (!val) return '—';
  const d = new Date(val);
  if (Number.isNaN(d.getTime())) return val;
  const pad = (n: number) => (n < 10 ? `0${n}` : `${n}`);
  return `${d.getFullYear()}/${pad(d.getMonth() + 1)}/${pad(d.getDate())} ${pad(d.getHours())}:${pad(d.getMinutes())}:${pad(d.getSeconds())}`;
};

const formatDisplayValue = (key: string, value: any) => {
  if (key === 'created_at') return formatDateTime(value);
  const mapping: Record<string, Record<string | number, string>> = {
    controller_gender: { 0: '女', 1: '男' },
    education_level: {
      0: '小学',
      1: '初中',
      1.5: '职高',
      2: '高中/中专',
      3: '大专',
      4: '本科',
      5: '硕士',
      6: '博士',
      7: '博士后',
    },
    marital_status: { 0: '未婚', 1: '已婚', 2: '离异', 3: '丧偶' },
    residence_type: { 0: '自购', 1: '租赁' },
    business_premises_type: { 0: '自有', 1: '租赁' },
    is_foreign_trade: { 0: '否', 1: '是' },
    is_cautious_industry: { 0: '否', 1: '是' },
    company_guarantee: { 0: '否', 1: '是' },
    personal_guarantee: { 0: '否', 1: '是' },
    additional_guarantor: { 0: '否', 1: '是' },
    is_growth_stage: { 0: '否', 1: '是' },
    used_youdaibao: { 0: '否', 1: '是' },
    family_harmony: { 0: '否', 1: '是' },
    industry_category: {
      0: '餐饮业',
      1: '纺织业',
      2: '服务业',
      3: '公安安全管理业',
      4: '建筑业',
      5: '教育业',
      6: '零售业',
      7: '贸易',
      8: '农业',
      9: '制造业',
    },
  };
  if (mapping[key] && mapping[key][value] !== undefined) {
    return mapping[key][value];
  }
  return formatValue(value);
};

const showModelOpinion = (record: RecordItem) => {
  modelOpinionText.value = (record?.prediction_text || '').toString().trim();
  modelOpinionVisible.value = true;
};


const deleteRecord = async (record: RecordItem) => {
  const id = record.id || record.project_number;
  if (!id) {
    message.error('????ID?????');
    return;
  }
  try {
    await axios.delete(`${API_NODE}/loan-application-with-summary/${id}`);
    message.success('删除成功');
    await fetchEntries();
  } catch (error: any) {
    console.error('删除失败', error);
    message.error(error?.response?.data?.error || '删除失败');
  }
};

const fetchEntries = async () => {
  if (!username.value) {
    message.warning('请先登录');
    return;
  }

  loading.value = true;
  try {
    const response = await axios.get(`${API_NODE}/loan-application`, {
      params: { createdBy: username.value },
    });
    records.value = response.data || [];
  } catch (error: any) {
    console.error('获取录入记录失败', error);
    message.error(error?.response?.data?.error || '获取录入记录失败');
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchEntries();
});


const cleanLabel = (label: string) => {
  // 去掉括号中的代码提示
  return label.replace(/（.*?）|\(.*?\)/g, '').trim();
};

const downloadRecord = async (record: RecordItem) => {
  const id = record.id || record.project_number;
  if (!id) {
    message.error('????ID?????');
    return;
  }

  try {
    const response = await axios.get(`${API_FLASK}/download-report`, {
      params: { id },
      responseType: 'blob',
    });

    const contentDisposition = response.headers['content-disposition'] || '';
    let filename = `${record.project_number || 'report'}-report.docx`;
    const match = /filename\*=UTF-8''([^;]+)|filename=\"?([^\";]+)\"?/i.exec(contentDisposition);
    if (match) {
      const rawName = match[1] || match[2];
      if (rawName) {
        filename = decodeURIComponent(rawName);
      }
    }

    const blob = new Blob([response.data], {
      type:
        response.headers['content-type'] ||
        'application/vnd.openxmlformats-officedocument.wordprocessingml.document',
    });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    a.click();
    URL.revokeObjectURL(url);
  } catch (error: any) {
    console.error('下载失败', error);
    message.error(error?.response?.data?.message || '下载失败');
  }
};

const handlePredict = async (record: RecordItem) => {
  const id = record.id || record.project_number;
  if (!id) {
    message.error('缺少记录ID，无法预测');
    return;
  }
  predictingId.value = id;
  try {
    // 调用后端预测接口
    const res = await axios.post(`${API_FLASK}/demo`, record);
    const predictedAmount =
      res.data?.['模型预测金额'] ??
      res.data?.model_result ??
      res.data?.predicted ??
      null;
    const llmJudgment =
      res.data?.['大模型判断'] ??
      res.data?.final_judgment ??
      res.data?.prediction_text ??
      '';

    // 将预测结果保存到数据库
    await axios.put(`${API_NODE}/loan-application/${id}`, {
      predicted: predictedAmount,
      prediction_text: llmJudgment,
      project_number: record.project_number,
    });

    // 先本地回填，避免刷新前界面看不到更新
    record.predicted = predictedAmount as any;
    record.prediction_text = llmJudgment as any;

    // 弹窗展示预测结果
    Modal.success({
      title: '预测完成',
      width: '80vw',
      content: h('div', [
        h('p', { style: 'font-size:16px;font-weight:600;color:#2b7a78;margin-bottom:8px;' },
          `模型预测额度: ${predictedAmount ?? '—'} 万元`),
        h('div', {
          style: 'height:70vh;overflow-y:auto;white-space:pre-wrap;background:#f7f9fa;padding:16px;border-radius:8px;font-size:14px;line-height:1.9;',
          innerHTML: (llmJudgment || '—').replace(/\n/g, '<br/>'),
        }),
      ]),
      okText: '确认并刷新',
      onOk() {
        window.location.reload();
      },
    });

    // 刷新列表
    await fetchEntries();
  } catch (error: any) {
    console.error('预测失败', error);
    message.error(error?.response?.data?.message || error?.response?.data?.error || '预测失败，请检查后端服务是否启动');
  } finally {
    predictingId.value = null;
  }
};

const parseJSON = (jsonStr: any) => {
  try {
    if (!jsonStr) return [];
    if (Array.isArray(jsonStr)) return jsonStr;
    if (typeof jsonStr === 'string') return JSON.parse(jsonStr);
    return [];
  } catch {
    return [];
  }
};

const normalizeSubjectName = (value: any) => (typeof value === 'string' ? value.trim() : '');

const groupExistingLoansBySubject = (loans: any[]) => {
  const groups: Array<{ subject_name: string; loans: any[] }> = [];
  const groupMap = new Map<string, { subject_name: string; loans: any[] }>();
  const hasLoanContent = (loan: any) =>
    ['type', 'amount', 'balance', 'mode', 'monthly_payment', 'start_date', 'end_date', 'bank_rate', 'purpose']
      .some((key) => {
        const value = loan?.[key];
        return value !== '' && value !== null && value !== undefined;
      });
  (Array.isArray(loans) ? loans : []).forEach((loan: any) => {
    if (!hasLoanContent(loan)) return;
    const subjectName = normalizeSubjectName(loan?.loan_subject) || '未命名贷款主体';
    if (!groupMap.has(subjectName)) {
      const subjectGroup = { subject_name: subjectName, loans: [] };
      groupMap.set(subjectName, subjectGroup);
      groups.push(subjectGroup);
    }
    groupMap.get(subjectName)!.loans.push(loan);
  });
  return groups;
};

const balanceMonthKeys = Array.from({ length: 12 }, (_, index) => `m${index + 1}`);

const toFiniteNumber = (value: any): number | null => {
  if (value === '' || value === null || value === undefined) {
    return null;
  }
  const parsed = Number(value);
  return Number.isFinite(parsed) ? parsed : null;
};

const formatSummaryNumber = (value: number): string =>
  value
    .toFixed(2)
    .replace(/\.00$/, '')
    .replace(/(\.\d)0$/, '$1');

const compareYearLabel = (a: string, b: string) => {
  const aNum = Number(a);
  const bNum = Number(b);
  if (Number.isFinite(aNum) && Number.isFinite(bNum)) {
    return aNum - bNum;
  }
  return a.localeCompare(b, 'zh-Hans-CN', { numeric: true });
};

const calculateAccountYearlyTotals = (rows: any[]) => {
  const totalsByYear = new Map<
    string,
    {
      year: string;
      monthlyTotals: Record<string, number>;
      monthHasValue: Record<string, boolean>;
    }
  >();

  rows.forEach((row: any) => {
    const year = String(row?.year ?? '').trim();
    if (!year) return;
    if (!totalsByYear.has(year)) {
      totalsByYear.set(year, {
        year,
        monthlyTotals: balanceMonthKeys.reduce((acc: Record<string, number>, key) => {
          acc[key] = 0;
          return acc;
        }, {}),
        monthHasValue: balanceMonthKeys.reduce((acc: Record<string, boolean>, key) => {
          acc[key] = false;
          return acc;
        }, {}),
      });
    }
    const yearTotal = totalsByYear.get(year)!;
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
      const totalRow: Record<string, any> = { year: entry.year, avg: '' };
      let hasAllMonths = true;
      let yearlySum = 0;
      let hasAnyMonth = false;
      balanceMonthKeys.forEach((key) => {
        if (!entry.monthHasValue[key]) {
          totalRow[key] = '';
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
    .filter((row): row is Record<string, any> => row !== null);
};

const getAccountBalanceGroups = (record: RecordItem) => {
  const accounts = parseJSON(record.business_accounts_json).map((acc: any, idx: number) => ({
    ...acc,
    account_id: acc?.account_id || `legacy_account_${idx}`,
    _index: idx,
  }));
  const rows = parseJSON(record.account_rows_json);
  const accountMap = new Map(accounts.map((acc: any) => [acc.account_id, acc]));
  const groups: Array<{ label: string; rows: any[] }> = [];
  const groupMap = new Map<string, { label: string; rows: any[] }>();

  rows.forEach((row: any) => {
    const hasMatch = row?.account_id && accountMap.has(row.account_id);
    const groupKey = hasMatch ? row.account_id : '__unlinked__';
    if (!groupMap.has(groupKey)) {
      if (hasMatch) {
        const acc = accountMap.get(row.account_id);
        groupMap.set(groupKey, {
          label: `账户${(acc?._index ?? 0) + 1}: ${acc?.account_name || '未填写开户银行'}${acc?.account_no ? ` - ${acc.account_no}` : ''}`,
          rows: [],
        });
      } else {
        groupMap.set(groupKey, {
          label: '未关联账户（历史数据）',
          rows: [],
        });
      }
      groups.push(groupMap.get(groupKey)!);
    }
    groupMap.get(groupKey)!.rows.push(row);
  });

  return groups;
};

const getAccountYearlyTotals = (record: RecordItem) =>
  calculateAccountYearlyTotals(parseJSON(record.account_rows_json));

const getExistingLoanSubjectGroups = (record: RecordItem) =>
  groupExistingLoansBySubject(parseJSON(record.existing_loans_json));
</script>

<style scoped>
.page {
  padding: 16px;
  background: #f5f7fa;
  min-height: calc(100vh - 64px);
}

.page-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.subtitle {
  margin: 4px 0 0;
  color: #666;
}

.actions {
  display: flex;
  align-items: center;
  gap: 12px;
  color: #666;
}

.count {
  font-weight: 600;
}

.detail-container {
  padding: 16px;
  background: #fafafa;
}

.detail-section {
  background: #ffffff;
  border-radius: 8px;
  padding: 16px;
  margin-bottom: 16px;
  border: 1px solid #e6dfd3;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.04);
}

.section-title {
  margin: 0 0 12px 0;
  padding-bottom: 8px;
  border-bottom: 2px solid #2b7a78;
  color: #235f68;
  font-size: 15px;
  font-weight: 600;
}

.detail-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
  gap: 12px;
}

.detail-item {
  padding: 8px 10px;
  background: #fafafa;
  border: 1px solid #f0f0f0;
  border-radius: 4px;
}

.detail-item.full-width {
  grid-column: 1 / -1;
}

.label {
  font-size: 12px;
  color: #888;
  margin-bottom: 4px;
}

.value {
  color: #1f1f1f;
  word-break: break-all;
}

.value.highlight {
  color: #2b7a78;
  font-weight: 600;
  font-size: 16px;
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
  font-size: 14px;
}

.sub-item {
  padding: 6px 0;
  border-bottom: 1px solid #e6e6e6;
  font-size: 13px;
  color: #333;
}

.sub-item:last-child {
  border-bottom: none;
}

.sub-item strong {
  color: #2b7a78;
}

.edit-form-container {
  max-height: 70vh;
  overflow-y: auto;
  padding-right: 8px;
}

.edit-form-container::-webkit-scrollbar {
  width: 8px;
}

.edit-form-container::-webkit-scrollbar-thumb {
  background: rgba(43, 122, 120, 0.35);
  border-radius: 10px;
}

.edit-section {
  background-color: #ffffff;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 16px;
  border: 1px solid #e6dfd3;
  box-shadow: 0 2px 8px rgba(20, 28, 34, 0.04);
}

.edit-section h3 {
  margin: 0 0 16px 0;
  color: #235f68;
  font-size: 16px;
  font-weight: 600;
  padding-bottom: 8px;
  border-bottom: 1px dashed #e6dfd3;
}

.edit-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 16px;
}

.edit-grid :deep(.ant-form-item) {
  margin-bottom: 0;
}

.edit-grid :deep(.ant-form-item-label) {
  padding-bottom: 4px;
}

.edit-grid :deep(.ant-form-item-label > label) {
  font-weight: 500;
  color: #6e7377;
  font-size: 14px;
}

.edit-grid :deep(.ant-input),
.edit-grid :deep(.ant-input-number),
.edit-grid :deep(.ant-select-selector),
.edit-grid :deep(.ant-input-number-input) {
  border-radius: 8px;
  border-color: #d9d3c8;
}

.edit-grid :deep(.ant-input:focus),
.edit-grid :deep(.ant-input-number-focused),
.edit-grid :deep(.ant-select-focused .ant-select-selector) {
  border-color: #2b7a78;
  box-shadow: 0 0 0 2px rgba(43, 122, 120, 0.1);
}

.edit-grid :deep(.ant-input-number) {
  width: 100%;
}

.edit-grid :deep(textarea.ant-input) {
  min-height: 80px;
  resize: vertical;
}

.sub-note {
  margin-top: 12px;
  padding: 8px 12px;
  background: #fff7e6;
  border-left: 3px solid #faad14;
  color: #ad6800;
  font-size: 13px;
  border-radius: 4px;
}

.model-opinion-content {
  max-height: 420px;
  overflow-y: auto;
  white-space: pre-wrap;
  background: #f7f9fa;
  padding: 12px;
  border-radius: 8px;
  font-size: 13px;
  line-height: 1.8;
}
</style>
