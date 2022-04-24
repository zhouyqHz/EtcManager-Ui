<template>
  <el-row :gutter="40" class="panel-group">
    <el-col :xs="12" :sm="12" :lg="8" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('newVisitis')">
        <div class="card-panel-icon-wrapper icon-people" style="margin-top:0px;margin-left:0px;padding-left:5px;padding-top:0px;">
          <el-row>
            <el-col :lg="4" >
              <svg-icon icon-class="phone" class-name="card-panel-icon" />
            </el-col>
            <el-col :lg="13">
              <div class="card-panel-description">
                <div class="card-panel-text">
                  OBU累计发放数量
                </div>
                <count-to :start-val="0" :end-val="1030" :duration="2600" class="card-panel-num" />
              </div>
            </el-col>
            <el-col :lg="7" >
              <el-descriptions :column="1" >
                <el-descriptions-item label="本日(个)">12</el-descriptions-item>
                <el-descriptions-item label="本月(个)">18</el-descriptions-item>
                <el-descriptions-item label="本年(个)">34</el-descriptions-item>
              </el-descriptions>
            </el-col>
          </el-row>
        </div>
      </div>
    </el-col>

    <el-col :xs="12" :sm="12" :lg="8" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('messages')">
        <div class="card-panel-icon-wrapper icon-message" style="margin-top:0px;margin-left:0px;padding-left:5px;padding-top:0px;">
          <el-row>
            <el-col :lg="4">
              <svg-icon icon-class="dict" class-name="card-panel-icon" />
            </el-col>

            <el-col :lg="13">
              <div class="card-panel-description">
                <div class="card-panel-text">
                  投放发放机数量
                </div>
                <count-to :start-val="0" :end-val="43" :duration="3000" class="card-panel-num" />
              </div>
            </el-col>

            <el-col :lg="7">
              <el-descriptions :column="1" >
                <el-descriptions-item label="本日(台)">12</el-descriptions-item>
                <el-descriptions-item label="本月(台)">18</el-descriptions-item>
                <el-descriptions-item label="本年(台)">34</el-descriptions-item>
              </el-descriptions>
            </el-col>
          </el-row>
        </div>
      </div>
    </el-col>

    <el-col :xs="12" :sm="12" :lg="8" class="card-panel-col">
      <div class="card-panel" @click="handleSetLineChartData('purchases')">
        <div class="card-panel-icon-wrapper icon-money" style="margin-top:0px;margin-left:0px;padding-left:5px;padding-top:0px;">
          <el-row>
            <el-col :lg="4">
              <svg-icon icon-class="documentation" class-name="card-panel-icon" />
            </el-col>

            <el-col :lg="13">
              <div class="card-panel-description">
                <div class="card-panel-text">
                  累计申请数量
                </div>
                <count-to :start-val="0" :end-val="1053" :duration="3200" class="card-panel-num" />
              </div>
            </el-col>

            <el-col :lg="7">
              <el-descriptions :column="1" >
                <el-descriptions-item label="本日(条)">12</el-descriptions-item>
                <el-descriptions-item label="本月(条)">18</el-descriptions-item>
                <el-descriptions-item label="本年(条)">34</el-descriptions-item>
              </el-descriptions>
            </el-col>
          </el-row>
        </div>
      </div>
    </el-col>
  </el-row>
</template>

<script>
import CountTo from 'vue-count-to'

export default {
  components: {
    CountTo
  },
  methods: {
    handleSetLineChartData(type) {
      this.$emit('handleSetLineChartData', type)
    }
  }
}
</script>

<style lang="scss" scoped>
.panel-group {
  margin-top: 18px;

  .card-panel-col {
    margin-bottom: 32px;
  }

  .card-panel:hover{
    outline-color: #5ca8ff;
  }
  .card-panel {
    height: 108px;
    cursor: pointer;
    font-size: 12px;
    position: relative;
    overflow: hidden;
    color: #666;
    background: #fff;
    box-shadow: 4px 4px 40px rgba(0, 0, 0, .05);
    /*border-color: rgba(0, 0, 0, .05);*/
    outline-style:solid;
    outline-color: #fff;
    transition: all 0.38s ease-out;

    &:hover {

    }

    .icon-people {
      color: #40c9c6;
    }

    .icon-message {
      color: #36a3f7;
    }

    .icon-money {
      color: #f4516c;
    }

    .icon-shopping {
      color: #34bfa3
    }

    .card-panel-icon-wrapper {
      float: left;
      margin: 14px 0 0 14px;
      padding: 16px;
      transition: all 0.38s ease-out;
      border-radius: 6px;
    }

    .card-panel-icon {
      float: left;
      font-size: 48px;
    }

    .card-panel-description {
      float: right;
      font-weight: bold;
      margin: 26px;
      margin-left: 0px;

      .card-panel-text {
        line-height: 18px;
        color: rgba(0, 0, 0, 0.45);
        font-size: 16px;
        margin-bottom: 12px;
      }

      .card-panel-num {
        font-size: 20px;
      }
    }
  }
}

@media (max-width:550px) {
  .card-panel-description {
    display: none;
  }

  .card-panel-icon-wrapper {
    float: none !important;
    width: 100%;
    height: 100%;
    margin: 0 !important;

    .svg-icon {
      display: block;
      margin: 14px auto !important;
      float: none !important;
    }
  }
}
</style>
