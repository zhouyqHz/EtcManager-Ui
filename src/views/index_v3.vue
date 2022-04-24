<template>
    <div>
      <div class="card-panel card-panel-top-right">
          <el-input
            placeholder="输入关键字进行过滤"
            v-model="filterText">
          </el-input>

        <el-tree
          class="filter-tree"
          :data="data"
          :props="defaultProps"
          :filter-node-method="filterNode"
          ref="tree"
          @node-click="nodeclick">
        </el-tree>
      </div>
      <!--发放机信息显示窗口-->
      <div class="card-panel card-panel-top-left" v-show="true">
        <div>
          <el-carousel height="150px">
            <el-carousel-item v-for="item in pic" :key="item">
              <img style="width: 100%;height: 100%;" :src="item" alt="" />
            </el-carousel-item>
          </el-carousel>

          <el-descriptions title="发放机信息" :column="1" border size="mini">
            <el-descriptions-item label="设备名称" label-class-name="my-label" content-class-name="my-content">OBU发放机</el-descriptions-item>
            <el-descriptions-item label="设备编号">0100</el-descriptions-item>
            <el-descriptions-item label="运行状态">
              <el-tag type="success" size="mini">正常</el-tag>
            </el-descriptions-item>
            <el-descriptions-item label="累计发放">105</el-descriptions-item>
            <el-descriptions-item label="剩余库存">
              <el-tag type="warning" size="mini">18</el-tag>
            </el-descriptions-item>
            <el-descriptions-item label="安装地址" :contentStyle="{'text-align': 'left'}">浙江省杭州市西湖区大龙驹坞705号</el-descriptions-item>
          </el-descriptions>
        </div>

      </div>
      <!-- 地图左下框     -->
      <div class="card-panel card-panel-bottom-right">
        <div>
          <el-drawer
            title="待处理信息"
            :visible.sync="drawer1"
            :direction="direction"
            :before-close="handleClose"
            append-to-body=true>
            <!--侧面弹出框数据表格            -->
            <el-table :data="tableData">
              <el-table-column property="id" label="序号" width="40"></el-table-column>
              <el-table-column property="eqipid" label="报警发放机" width="60"></el-table-column>
              <el-table-column property="alertcontent" label="报警内容" width="80"></el-table-column>
              <el-table-column property="time" label="时间" ></el-table-column>
              <el-table-column property="address" label="地点" width="200"></el-table-column>
              <el-table-column label="操作">
                <el-button size="small" type="text" icon="el-icon-thumb">处理</el-button>
                <el-button size="mini" type="text" icon="el-icon-brush">忽略</el-button>
              </el-table-column>
            </el-table>
          </el-drawer>

          <el-drawer
            title="未读信息"
            :visible.sync="drawer2"
            :direction="direction"
            :before-close="handleClose"
            append-to-body=true>
            <!--侧面弹出框数据表格            -->
            <el-table :data="tableData">
              <el-table-column property="id" label="序号" width="40"></el-table-column>
              <el-table-column property="eqipid" label="报警发放机" width="60"></el-table-column>
              <el-table-column property="alertcontent" label="报警内容" width="80"></el-table-column>
              <el-table-column property="time" label="时间" ></el-table-column>
              <el-table-column property="address" label="地点" width="200"></el-table-column>
              <el-table-column label="操作">
                <el-button size="small" type="text" icon="el-icon-thumb">处理</el-button>
                <el-button size="mini" type="text" icon="el-icon-brush">忽略</el-button>
              </el-table-column>
            </el-table>
          </el-drawer>

          <el-drawer
            title="正在处理"
            :visible.sync="drawer3"
            :direction="direction"
            :before-close="handleClose"
            append-to-body=true>
            <!--侧面弹出框数据表格            -->
            <el-table :data="handledata">
              <el-table-column property="id" label="序号" width="40">
                <template slot-scope="scope">{{scope.row.id}}</template>
              </el-table-column>
              <el-table-column property="data" label="操作时间" width="100"></el-table-column>
              <el-table-column property="status" label="当前进程" >
                <template slot-scope="scope">
                  <el-steps :active="scope.row.status" finish-status="success">
                    <el-step title="正在报警"></el-step>
                    <el-step title="正在处理"></el-step>
                    <el-step title="寻求支援"></el-step>
                    <el-step title="报警解除"></el-step>
                  </el-steps>
                </template>
              </el-table-column>
              <el-table-column property="handlepeople" label="操作员" width="100"></el-table-column>
            </el-table>
          </el-drawer>
        </div>
        <div>
          <el-badge :value="2" class="item" style="margin-right:20px;">
            <el-button size="small" style="background: #ffaf44" @click="drawer1 = true">待处理</el-button>
          </el-badge>
          <el-badge :value="2" class="item" style="margin-right:20px;">
            <el-button size="small" style="background: #49ffca" @click="drawer2 = true">未读消息</el-button>
          </el-badge>
          <el-badge :value="5" class="item" style="margin-right:20px;">
            <el-button size="small" style="background: #86ffa8" @click="drawer3 = true">正在处理</el-button>
          </el-badge>
        </div>

      </div>
      <!-- 地图     -->
      <div id="map" style="height: 0;padding-bottom: 52%"></div>
    </div>
</template>

<script>
  import PieChart from './dashboard/PieChart';
  import { constantRoutes } from "@/router";
  import pic1 from "../assets/images/1.jpg";
  import pic2 from "../assets/images/2.jpg";
  import pic3 from "../assets/images/3.jpg";

  export default {
    name: 'index_v3',
    components: {
      PieChart,
    },
    // 数据过滤相关
    watch: {
      filterText(val) {
        this.$refs.tree.filter(val);
      }
    },
    methods: {
      filterNode(value, data) {
        if (!value) return true;
        return data.label.indexOf(value) !== -1;
      },
      //树节点点击函数，有点不懂，为啥这个data可以直接使用
      nodeclick(data){
        // 为啥map加this后就能调用了
        var center = this.map.getCenter();
        this.map.flyTo({
          center: this.centerlist[data.label],
          zoom: 12,
          bearing: 10,
          pitch: 30,
          duration: 2000
        });
        this.infocenter = data.label;
        this.equiinfohide = true;
      },
      // 绘制标记点函数
      drawpoint(pointlist){
        var cen = pointlist;
        // var cen = this.centerlist;
        // 循环画各个区的点
          for (var key in cen){
            if(this.map){
              // 生成自定义点元素
              var el = document.createElement('div');
              el.id = 'marker';
              el.style["background-image"]="url(https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.199it.com%2Fwp-content%2Fuploads%2F2016%2F09%2Funnamed.png&refer=http%3A%2F%2Fwww.199it.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1651988644&t=f1c125b294c0971d179cc4eced9155f3)";
              el.style["background-size"]="100% 100%"; // 设置背景自适应
              el.style.width = "35px";
              el.style.height = "35px";
              // 使用自定义元素生成点
              var _marker = new minemap.Marker(el, {offset: [-20, -50]}).setLngLat(cen[key]).addTo(this.map);
              this.markers.push(_marker);
              // 监听点击
              $(el).on('click', function(e){
                  console.log('ok');
              })
            }
          }
      },
      //根据放大倍数显示点
      updatapoint(){
        var zoom = this.map.getZoom();
        // 放大到指定倍数，删除点
        if(zoom>=12){
          if (this.map && this.markers.length !== 0) {
            for (let i = 0; i < this.markers.length; i++) {
              this.markers[i].remove();
            }
          }
          this.drawpoint(this.centerlisttwo);
        }
        // 缩小到指定倍数，就画点
        else {
          if (this.map && this.markers.length !== 0) {
            for (let i = 0; i < this.markers.length; i++) {
              this.markers[i].remove();
            }
          }
          this.drawpoint(this.centerlist);
        }
      }

    },
    data() {
      return{
        pic: [pic1, pic2, pic3],
        filterText: '',
        infocenter: '杭州市',
        drawer1: false,
        drawer2: false,
        drawer3: false,
        markers: [],
        equiinfohide: false,
        centerlist:{
          "临安区":[119.724618,30.242052],
          "西湖区":[120.122643,30.243706],
          "拱墅区":[120.147364,30.328531],
          "上城区":[120.199682,30.235219],
          "滨江区":[120.221241,30.207757],
          "萧山区":[120.271834,30.186781]},
        centerlisttwo:{
          "西湖区1":[120.08578,30.248167],
          "西湖区2":[120.075042,30.250945],
          "西湖区3":[120.07073,30.24703],
          "西湖区4":[120.020245,30.213408],
        },
        data: [{
          id: 1,
          label: '杭州市(30)',
          children: [{
            id: 4,
            label: '西湖区',
          },
            {
              id: 2,
              label: '上城区',
            },
            {
              id: 3,
              label: '滨江区',
            },
            {
              id: 4,
              label: '拱墅区',
            },
            {
              id: 5,
              label: '萧山区',
            },
            {
              id: 6,
              label: '临安区',
            }]
        }],
        // 待处理数据
        tableData:[
          {
            id: '1',
            eqipid: '0029',
            alertcontent: '缺货',
            time: '2022-04-04',
            address: '浙江省西湖区留下街道大龙驹坞705号',
          }
          ],
        // 处理中数据
        handledata:[
          {id: '1', data: '2022-04-15 14:22', status: '2', handlepeople: '武大郎'},
          {id: '2', data: '2022-04-14 11:56', status: '1', handlepeople: '林喵喵'},
          {id: '3', data: '2022-04-13 08:12', status: '4', handlepeople: '李大勇'},
        ],
      }
    },
    mounted() {
      /**
       * 初始化地图实例,放在mounted中，网页加载完成后进行地图加载
       */
      this.map = new minemap.Map({
        container: 'map',
        style: 'https://minedata.cn/service/solu/style/id/12879', /*底图样式*/
        center: [120.098802,30.260319], /*地图中心点*/
        zoom: 10, /*地图默认缩放等级*/
        pitch: 0, /*地图俯仰角度*/
        maxZoom: 17, /*地图最大缩放等级*/
        minZoom: 3,  /*地图最小缩放等级*/
        projection: 'MERCATOR'
      });
      // 事件绑定
      this.map.on('load', this.drawpoint(this.centerlist));
      this.map.on('zoomend', this.updatapoint);
    }
  }
</script>

<style scoped>

</style>
