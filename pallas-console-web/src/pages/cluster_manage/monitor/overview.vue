<template>
    <div class="page-content">
        <div class="my-breadcrumb">
            <el-breadcrumb separator="/" class="my-breadcrumb-content">
                <el-breadcrumb-item :to="{ name:'cluster_manage' }"><i class="fa fa-home"></i>ES集群管理</el-breadcrumb-item>
                <el-breadcrumb-item :to="{ name:'cluster_detail', query: { clusterId } }">{{clusterId}}</el-breadcrumb-item>
                <el-breadcrumb-item :to="{ name:'cluster_monitor', query: { clusterId } }">监控</el-breadcrumb-item>
                <el-breadcrumb-item :to="item.route" v-for="(item, index) in breadcrumbs" :key="index">{{item.name}}</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="page-tab">
            <el-tabs v-model="activeTab" @tab-click="onTabClick">
                <el-tab-pane name="cluster_monitor">
                    <span slot="label"><i class="fa fa-cube"></i>集群</span>
                </el-tab-pane>
                <el-tab-pane name="indices_monitor">
                    <span slot="label"><i class="fa fa-search"></i>索引 (120)</span>
                </el-tab-pane>
                <el-tab-pane name="nodes_monitor">
                    <span slot="label"><i class="fa fa-cubes"></i>节点 (3)</span>
                </el-tab-pane>
            </el-tabs>
            <router-view></router-view>
        </div>
    </div>
</template>

<script>

export default {
  data() {
    return {
      activeTab: 'cluster_monitor',
    };
  },
  methods: {
    onTabClick() {
      this.$router.push({
        name: this.activeTab,
        query: {
          clusterId: this.clusterId,
        },
      });
    },
    getActiveTab() {
      const str = this.$route.name;
      if (str) {
        if (str === 'indice_monitor_detail') {
          this.activeTab = 'indices_monitor';
        } else if (str === 'node_monitor_detail') {
          this.activeTab = 'nodes_monitor';
        } else {
          this.activeTab = str;
        }
      }
    },
  },
  computed: {
    clusterId() {
      return this.$route.query.clusterId;
    },
    indice() {
      return this.$route.query.indice;
    },
    node() {
      return this.$route.query.node;
    },
    breadcrumbs() {
      const result = [];
      if (this.$route.name === 'cluster_monitor') {
        result.push({ name: '集群' });
      } else if (this.$route.name === 'indices_monitor') {
        result.push({ name: '索引' });
      } else if (this.$route.name === 'indice_monitor_detail') {
        result.push({ name: '索引', route: { name: 'indices_monitor', query: { clusterId: this.clusterId } } });
        result.push({ name: this.indice });
      } else if (this.$route.name === 'nodes_monitor') {
        result.push({ name: '节点' });
      } else if (this.$route.name === 'node_monitor_detail') {
        result.push({ name: '节点', route: { name: 'nodes_monitor', query: { clusterId: this.clusterId } } });
        result.push({ name: this.node });
      }
      return result;
    },
  },
  created() {
    this.getActiveTab();
  },
  watch: {
    $route: 'getActiveTab',
  },
};
</script>
