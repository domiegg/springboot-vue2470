<template>
<div>
	<div :style='{"padding":"12px","margin":"10px auto","borderColor":"#dbd9f4","borderRadius":"8px","background":"#fff","borderWidth":"0 0 2px","width":"1200px","borderStyle":"solid"}' class="breadcrumb-preview">
		<el-breadcrumb :separator="'Ξ'" :style='{"width":"100%","margin":"0 auto","fontSize":"14px","lineHeight":"1","display":"flex"}'>
			<el-breadcrumb-item>首页</el-breadcrumb-item>
			<el-breadcrumb-item v-for="(item, index) in breadcrumbItem" :key="index">{{item.name}}</el-breadcrumb-item>
		</el-breadcrumb>
	</div>
	
	<div class="list-preview" :style='{"width":"1200px","margin":"10px auto","position":"relative","background":"none"}'>
		<div class="category-1" :style='{"padding":"10px","borderColor":"#dbd9f4","borderRadius":"8px","background":"#fff","borderWidth":"2px 1px 1px 1px","display":"flex","width":"100%","borderStyle":"solid","height":"auto"}'>
			<div class="item" :class="swiperIndex == '-1' ? 'active' : ''" @click="getList(1, '全部')" :plain="isPlain">全部</div>
			<div class="item" :class="swiperIndex == index ? 'active' : ''" v-for="(item, index) in fenlei" :key="index" @click="getList(1, item, 'btn' + index)" :ref="'btn' + index" plain>{{item}}</div>
		</div>
		
	
    <el-form :inline="true" :model="formSearch" class="list-form-pv" :style='{"border":"2px solid #dbd9f4","padding":"10px","margin":"10px 0 0 0","alignItems":"center","borderRadius":"8px","flexWrap":"wrap","background":"#f8f8fc","display":"flex","width":"100%","height":"auto"}'>
      <el-form-item :style='{"margin":"10px"}'>
	    <div class="lable" v-if="true" :style='{"width":"auto","padding":"0 10px","lineHeight":"42px","display":"inline-block"}'>菜谱名称</div>
        <el-input v-model="formSearch.caipumingcheng" placeholder="菜谱名称" clearable></el-input>
      </el-form-item>
      <el-form-item :style='{"margin":"10px"}'>
	    <div class="lable" v-if="true" :style='{"width":"auto","padding":"0 10px","lineHeight":"42px","display":"inline-block"}'>口味</div>
        <el-input v-model="formSearch.kouwei" placeholder="口味" clearable></el-input>
      </el-form-item>
      <el-form-item :style='{"margin":"10px"}'>
	    <div class="lable" v-if="true" :style='{"width":"auto","padding":"0 10px","lineHeight":"42px","display":"inline-block"}'>适应人群</div>
        <el-input v-model="formSearch.shiyingrenqun" placeholder="适应人群" clearable></el-input>
      </el-form-item>
      <el-form-item :style='{"margin":"10px"}'>
	    <div class="lable" v-if="true" :style='{"width":"auto","padding":"0 10px","lineHeight":"42px","display":"inline-block"}'>功效营养</div>
        <el-input v-model="formSearch.gongxiaoyingyang" placeholder="功效营养" clearable></el-input>
      </el-form-item>
	  <el-button v-if=" true " :style='{"cursor":"pointer","border":"0","padding":"0px 15px","margin":"0 10px 0 0","outline":"none","color":"#666","borderRadius":"6px","background":"linear-gradient(90deg, rgba(255,233,100,1) 0%, rgba(194,248,126,1) 29%, rgba(181,233,252,1) 61%, rgba(246,172,218,1) 100%)","width":"auto","fontSize":"14px","lineHeight":"42px","height":"42px"}' type="primary" @click="getList(1, curFenlei)"><i v-if="true" :style='{"color":"#666","margin":"0 10px 0 0","fontSize":"14px"}' class="el-icon-search"></i>查询</el-button>
	  <el-button v-if="isAuth('caipuxinxi','新增')" :style='{"cursor":"pointer","border":"0","padding":"0px 15px","margin":"0 10px 0 0","outline":"none","color":"#666","borderRadius":"6px","background":"linear-gradient(90deg, rgba(255,233,100,1) 0%, rgba(194,248,126,1) 29%, rgba(181,233,252,1) 61%, rgba(246,172,218,1) 100%)","width":"auto","fontSize":"14px","lineHeight":"42px","height":"42px"}' type="primary" @click="add('/index/caipuxinxiAdd')"><i v-if="true" :style='{"color":"#666","margin":"0 10px 0 0","fontSize":"14px"}' class="el-icon-circle-plus-outline"></i>添加</el-button>
    </el-form>

	<div class="list" :style='{"border":"2px solid #dbd9f4","margin":"20px 0 0 0","borderRadius":"8px","background":"#fff"}'>
		<!-- 样式一 -->
		
		<!-- 样式二 -->
		<div class="list2 index-pv1" :style='{"width":"100%","padding":"20px 32px 20px 20px","flexWrap":"wrap","justifyContent":"space-between","display":"flex","height":"auto"}'>
			<div :style='{"cursor":"pointer","padding":"0","margin":"0 0 20px 0","borderRadius":"8px","background":"#f6f6f6","display":"inline-block","width":"23%","fontSize":"0","position":"relative","height":"auto"}' v-for="(item, index) in dataList" :key="index" @click="toDetail(item)" class="list-item animation-box">
				<img :style='{"width":"calc(100% - 24px)","margin":"12px","objectFit":"cover","borderRadius":"8px","display":"inline-block","height":"240px"}' v-if="item.caipufengmian && item.caipufengmian.substr(0,4)=='http'" :src="item.caipufengmian" class="image" />
				<img :style='{"width":"calc(100% - 24px)","margin":"12px","objectFit":"cover","borderRadius":"8px","display":"inline-block","height":"240px"}' v-else :src="baseUrl + (item.caipufengmian?item.caipufengmian.split(',')[0]:'')" class="image" />
				<div class="item-info" :style='{"padding":"0px","overflow":"hidden","borderRadius":"0 0 8px 8px","background":"#f6f6f6","display":"inline-block","width":"100%","height":"auto"}'>
					<div :style='{"padding":"0 0 0 12px","margin":"0px 24px 4px 0","whiteSpace":"nowrap","overflow":"hidden","color":"#333","borderRadius":"0 20px 20px 0","background":"#ede9f6","lineHeight":"32px","fontSize":"14px","textOverflow":"ellipsis"}' class="name ">菜谱名称:{{item.caipumingcheng}}</div>
					<div :style='{"padding":"0 0 0 12px","margin":"0px 24px 4px 0","whiteSpace":"nowrap","overflow":"hidden","color":"#333","borderRadius":"0 20px 20px 0","background":"#ede9f6","lineHeight":"32px","fontSize":"14px","textOverflow":"ellipsis"}' class="name ">{{item.caipinfenlei}}</div>
					<div :style='{"padding":"0 0 0 12px","margin":"0px 24px 4px 0","whiteSpace":"nowrap","overflow":"hidden","color":"#333","borderRadius":"0 20px 20px 0","background":"#ede9f6","lineHeight":"32px","fontSize":"14px","textOverflow":"ellipsis"}' class="name ">{{item.kouwei}}</div>
					<div v-if="item.price" :style='{"padding":"0px","margin":"0 12px 4px 0","lineHeight":"24px","fontSize":"12px","color":"#d15858","textAlign":"right"}' class="price"><span :style='{"fontSize":"12px"}'>￥</span>{{item.price}}</div>
				</div>
			</div>
		</div>
	</div>

	<!-- 热门信息 -->
	<div class="hot" :style='{"border":"2px solid #dbd9f4","margin":"20px 0 0","borderRadius":"8px","background":"#fff","width":"100%","position":"relative","height":"auto"}'>
	  <div :style='{"padding":"0 20px","margin":"0 auto","color":"#000","textAlign":"center","background":"url(http://codegen.caihongy.cn/20221105/bb1ea9437beb4e1da8fcd1583db2f111.png) no-repeat,radial-gradient(circle, rgba(219,217,244,1) 0%, rgba(181,177,240,1) 100%)","width":"260px","lineHeight":"42px","fontSize":"24px","height":"42px"}'>热门信息</div>
	  <div :style='{"padding":"20px","margin":"4px 0 0 0","background":"none","display":"flex","width":"100%","justifyContent":"space-between","height":"auto"}'>
	    <div v-for="item in hotList" :key="item" :style='{"cursor":"pointer","width":"23%","padding":"0 0 8px 0","borderRadius":"8px","background":"#f6f6f6","height":"auto"}' @click="toDetail(item)">
	      <img :style='{"width":"calc(100% - 24px)","margin":"12px","objectFit":"cover","borderRadius":"8px","display":"block","height":"240px"}' :src="baseUrl + (item.caipufengmian?item.caipufengmian.split(',')[0]:'')" alt="">
	      <div :style='{"padding":"0 0 0 12px","margin":"0px 24px 4px 0","color":"#333","borderRadius":"0 20px 20px 0","background":"#ede9f6","lineHeight":"32px","fontSize":"14px"}'>{{item.caipumingcheng}}</div>
	      <div :style='{"padding":"0 0 0 12px","margin":"0px 24px 4px 0","color":"#333","borderRadius":"0 20px 20px 0","background":"#ede9f6","lineHeight":"32px","fontSize":"14px"}'>{{item.caipinfenlei}}</div>
	      <div :style='{"padding":"0 0 0 12px","margin":"0px 24px 4px 0","color":"#333","borderRadius":"0 20px 20px 0","background":"#ede9f6","lineHeight":"32px","fontSize":"14px"}'>{{item.kouwei}}</div>
	      <!--<div :style='{"padding":"0 10px","lineHeight":"24px","fontSize":"12px","color":"#999","textAlign":"right","background":"none"}'>2022-02-02</div>-->
	    </div>
	  </div>
	</div>
	
	<el-pagination
	  background
	  class="pagination"
	  :pager-count="7"
	  :page-size="pageSize"
	  :page-sizes="pageSizes"
	  prev-text="<"
	  next-text=">"
	  :hide-on-single-page="true"
	  :layout='["total","prev","pager","next","sizes","jumper"].join()'
	  :total="total"
	  :style='{"width":"1200px","padding":"0","margin":"20px auto","whiteSpace":"nowrap","color":"#333","fontWeight":"500"}'
	  @current-change="curChange"
	  @prev-click="prevClick"
	  @next-click="nextClick"
	></el-pagination>

  </div>
</div>
</template>

<script>
  export default {
    //数据集合
    data() {
      return {
	    layouts: '',
		swiperIndex: -1,
        baseUrl: '',
        breadcrumbItem: [
          {
            name: '菜谱信息'
          }
        ],
        formSearch: {
          caipumingcheng: '',
          kouwei: '',
          shiyingrenqun: '',
          gongxiaoyingyang: '',
        },
        fenlei: [],
        hotList: [],
        dataList: [],
        total: 1,
        pageSize:  10,
		pageSizes: [10,20,30,50],
        totalPage: 1,
        curFenlei: '全部',
        isPlain: false,
        indexQueryCondition: '',
        timeRange: []
      }
    },
    created() {
      this.indexQueryCondition = this.$route.query.indexQueryCondition ? this.$route.query.indexQueryCondition : '';
      this.baseUrl = this.$config.baseUrl;
      this.getFenlei();
      this.getList(1, '全部');
      this.getHotList();
    },
    //方法集合
    methods: {
      add(path) {
        this.$router.push({path: path});
      },
      getHotList() {
        let autoSortUrl = "";
        autoSortUrl = "caipuxinxi/autoSort";
        if(localStorage.getItem('Token')) {
            autoSortUrl = "caipuxinxi/autoSort2";
        }
          this.$http.get(autoSortUrl, {params: {
              page: 1,
              limit: 4,
          }}).then(res => {
              if (res.data.code == 0) {
                  this.hotList = res.data.data.list;
              }
          })
      },
      getFenlei() {
        this.$http.get('option/caipinfenlei/caipinfenlei').then(res => {
          if (res.data.code == 0) {
            this.fenlei = res.data.data;
          }
        });
      },
      getList(page, fenlei, ref = '') {
		if(fenlei == '全部') this.swiperIndex = -1;
		for(let i=0;i<this.fenlei.length;i++) {
			if(fenlei == this.fenlei[i]) {
				this.swiperIndex = i;
				break;
			}
		}
        this.curFenlei = fenlei;
        if (this.curFenlei == '全部') {
          this.isPlain = false;
        } else {
          this.isPlain = true;
        }
        let params = {page, limit: this.pageSize};
        let searchWhere = {};
        if (this.formSearch.caipumingcheng != '') searchWhere.caipumingcheng = '%' + this.formSearch.caipumingcheng + '%';
        if (this.formSearch.kouwei != '') searchWhere.kouwei = '%' + this.formSearch.kouwei + '%';
        if (this.formSearch.shiyingrenqun != '') searchWhere.shiyingrenqun = '%' + this.formSearch.shiyingrenqun + '%';
        if (this.formSearch.gongxiaoyingyang != '') searchWhere.gongxiaoyingyang = '%' + this.formSearch.gongxiaoyingyang + '%';
        if (this.curFenlei != '全部') searchWhere.caipinfenlei = this.curFenlei;
        this.$http.get('caipuxinxi/list', {params: Object.assign(params, searchWhere)}).then(res => {
          if (res.data.code == 0) {
            this.dataList = res.data.data.list;
            this.total = res.data.data.total;
            this.pageSize = res.data.data.pageSize;
            this.totalPage = res.data.data.totalPage;
			
			this.pageSizes = [this.pageSize, this.pageSize*2, this.pageSize*3, this.pageSize*5];
          }
        });
      },
      curChange(page) {
        this.getList(page,this.curFenlei);
      },
      prevClick(page) {
        this.getList(page,this.curFenlei);
      },
      nextClick(page) {
        this.getList(page,this.curFenlei);
      },
      toDetail(item) {
        this.$router.push({path: '/index/caipuxinxiDetail', query: {detailObj: JSON.stringify(item)}});
      },
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
	.list-preview .list-form-pv .el-input {
		width: auto;
	}

	.breadcrumb-preview .el-breadcrumb /deep/ .el-breadcrumb__separator {
		margin: 0 9px;
		color: #ccc;
		font-weight: 500;
	}
	
	.breadcrumb-preview .el-breadcrumb /deep/ .el-breadcrumb__inner a {
		color: #333;
		display: inline-block;
	}
	
	.breadcrumb-preview .el-breadcrumb /deep/ .el-breadcrumb__inner {
		color: #666;
		display: inline-block;
	}
	
	.category-1 .item {
		cursor: pointer;
		border: 1px solid #dbd9f4;
		border-radius: 4px;
		margin: 0 10px 0 0;
		color: #666;
		background: linear-gradient(0deg, rgba(255,255,255,1) 20%, rgba(198,163,235,1) 100%);
		display: flex;
		width: 72px;
		font-size: 14px;
		line-height: 36px;
		justify-content: center;
		text-align: center;
	}
	
	.category-1 .item:hover {
		cursor: pointer;
		border-radius: 4px;
		margin: 0 10px 0 0;
		color: #333;
		background: linear-gradient(180deg, rgba(255,255,255,1) 25%, rgba(198,163,235,1) 100%);
		width: 72px;
		font-size: 14px;
		line-height: 36px;
		text-align: center;
	}
	
	.category-1 .item.active {
		cursor: pointer;
		border-radius: 4px;
		margin: 0 10px 0 0;
		color: #333;
		background: linear-gradient(180deg, rgba(255,255,255,1) 25%, rgba(198,163,235,1) 100%);
		width: 72px;
		font-size: 14px;
		line-height: 36px;
		text-align: center;
	}
	
	.category-2 .item {
		cursor: pointer;
		border-radius: 4px;
		margin: 0 0 10px 0;
		color: #999;
		background: #efefef;
		width: 100%;
		font-size: 14px;
		line-height: 36px;
		text-align: center;
	}
	
	.category-2 .item:hover {
		cursor: pointer;
		border-radius: 4px;
		margin: 0 0 10px 0;
		color: #999;
		background: #efefef;
		width: 100%;
		font-size: 14px;
		line-height: 36px;
		text-align: center;
	}
	
	.category-2 .item.active {
		cursor: pointer;
		border-radius: 4px;
		margin: 0 0 10px 0;
		color: #999;
		background: #efefef;
		width: 100%;
		font-size: 14px;
		line-height: 36px;
		text-align: center;
	}
	
	.list-form-pv .el-input /deep/ .el-input__inner {
		border: 1px solid #dbd9f4;
		border-radius: 8px;
		padding: 0 10px;
		margin: 0;
		outline: none;
		color: #333;
		width: 140px;
		font-size: 14px;
		line-height: 42px;
		height: 42px;
	}
	
	.list-form-pv .el-select /deep/ .el-input__inner {
		border: 1px solid #dbd9f4;
		border-radius: 8px;
		padding: 0 10px;
		margin: 0;
		outline: none;
		color: #333;
		width: 140px;
		font-size: 14px;
		line-height: 42px;
		height: 42px;
	}
	
	.list-form-pv .el-date-editor /deep/ .el-input__inner {
		border: 1px solid #dbd9f4;
		border-radius: 8px;
		padding: 0 30px;
		margin: 0;
		outline: none;
		color: #333;
		width: 140px;
		font-size: 14px;
		line-height: 42px;
		height: 42px;
	}
	
	.list .index-pv1 .animation-box {
		transform: rotate(0deg) scale(1) skew(0deg, 0deg) translate3d(0px, 0px, 0px);
		z-index: initial;
	}
	
	.list .index-pv1 .animation-box:hover {
		transform: rotate(0) scale(0.98) skew(0deg, 0deg) translate3d(0px, 0px, 0px);
		-webkit-perspective: 1000px;
		perspective: 1000px;
		transition: 0.3s;
		z-index: 1;
	}
	
	.list .index-pv1 .animation-box img {
		transform: rotate(0deg) scale(1) skew(0deg, 0deg) translate3d(0px, 0px, 0px);
	}
	
	.list .index-pv1 .animation-box img:hover {
		transform: 0;
		-webkit-perspective: 1000px;
		perspective: 1000px;
		transition: 0.3s;
	}
	
	.el-pagination /deep/ .el-pagination__total {
		margin: 0 10px 0 0;
		color: #666;
		font-weight: 400;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .btn-prev {
		border: none;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px 0 40%;
		color: #999;
		background: #fff;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		min-width: 35px;
		height: 28px;
	}
	
	.el-pagination /deep/ .btn-next {
		border: none;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #999;
		background: #fff;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		min-width: 35px;
		height: 28px;
	}
	
	.el-pagination /deep/ .btn-prev:disabled {
		border: none;
		cursor: not-allowed;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #C0C4CC;
		background: #fff;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .btn-next:disabled {
		border: none;
		cursor: not-allowed;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #C0C4CC;
		background: #fff;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pager {
		padding: 0;
		margin: 0;
		display: inline-block;
		vertical-align: top;
	}
	
	.el-pagination /deep/ .el-pager .number {
		cursor: pointer;
		padding: 0 4px;
		margin: 0 5px;
		color: #999;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 2px;
		background: #fff;
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pager .number:hover {
		cursor: pointer;
		padding: 0 4px;
		margin: 0 5px;
		color: #666;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 2px;
		background: rgba(171, 133, 211,.2);
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pager .number.active {
		cursor: default;
		padding: 0 4px;
		margin: 0 5px;
		color: #FFF;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 2px;
		background: rgba(171, 133, 211,.5);
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__sizes {
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__sizes .el-input {
		margin: 0 5px;
		width: 100px;
		position: relative;
	}
	
	.el-pagination /deep/ .el-pagination__sizes .el-input .el-input__inner {
		border: 1px solid #DCDFE6;
		cursor: pointer;
		padding: 0 25px 0 8px;
		color: #606266;
		display: inline-block;
		font-size: 13px;
		line-height: 28px;
		border-radius: 3px;
		outline: 0;
		background: #FFF;
		width: 100%;
		text-align: center;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__sizes .el-input span.el-input__suffix {
		top: 0;
		position: absolute;
		right: 0;
		height: 100%;
	}
	
	.el-pagination /deep/ .el-pagination__sizes .el-input .el-input__suffix .el-select__caret {
		cursor: pointer;
		color: #C0C4CC;
		width: 25px;
		font-size: 14px;
		line-height: 28px;
		text-align: center;
	}
	
	.el-pagination /deep/ .el-pagination__jump {
		margin: 0 0 0 24px;
		color: #606266;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__jump .el-input {
		border-radius: 3px;
		padding: 0 2px;
		margin: 0 2px;
		display: inline-block;
		width: 50px;
		font-size: 14px;
		line-height: 18px;
		position: relative;
		text-align: center;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__jump .el-input .el-input__inner {
		border: 1px solid #DCDFE6;
		cursor: pointer;
		padding: 0 3px;
		color: #606266;
		display: inline-block;
		font-size: 14px;
		line-height: 28px;
		border-radius: 3px;
		outline: 0;
		background: #FFF;
		width: 100%;
		text-align: center;
		height: 28px;
	}
</style>
