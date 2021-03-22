<template>
  <div class="home">
    <!-- 上边黄框 开始-->
    <el-container class="head-container">

      <!-- 标题+logo 开始-->
      <el-container class="left">
        <img src="../assets/Home/logo3.png" alt="" style="height:70px;width:70px;margin-right:20px;margin-left:20px;">
        <h1 style="font-family:'PingFang SC';
                  font-weight:bold;
                  color:#1C3263;
                  font-size:130%;">共享租赁平台</h1>
      </el-container>
      <!-- 标题+logo 结束-->

      <!-- 中间框 开始-->
      <el-container class="middle">
        <!-- 搜索栏 -->
        <div style="width:80%;">
          <el-input placeholder="请输入内容"
                    prefix-icon="el-icon-search"
                    v-model="input1">
          </el-input>
        </div>
        <!-- 热门搜索建议 -->
        <div style="width: 80%;
                  margin-top: 8px;
                  display: flex;
                  flex-direction: row;
                  justify-content: space-between;
                  font-size:12px;
                  font-weight:bold;
                  align-items: center;">
          <span>小皮鞋</span>
          <span>三角钢琴</span>
          <span>女士礼服</span>
          <span>高定礼服</span>
          <span>珍珠项链</span>
          <span>西装外套</span>
          <span>更多...</span>
        </div>
      </el-container>
      <!-- 中间框 结束-->

      <!-- 头像 + 登录/注册 + 购物车图标 + 收藏图标 开始-->
      <el-container class="right">
        <img src="../assets/Home/图片@1x.png" alt="" style="width:60px;height:60px;margin-right:20px;">
        <span style="font-weight:bold;">登录/注册</span>
        <img src="../assets/Home/shopping-cart.png" alt="" style="height:30px; width:30px; margin-left:20px;">
        <img src="../assets/Home/like.png" alt="" style="height:30px; width:30px; margin-left:16px;">
      </el-container>
      <!-- 头像 + 登录/注册 + 购物车图标 + 收藏图标 结束-->

    </el-container>
    <!-- 上边黄框 结束-->

    <!-- 轮播图 -->
    <el-carousel indicator-position="none">
      <el-carousel-item v-for="index in 4" :key="index">
        <img src="../assets/Home/轮播1.png" v-if="index % 2 == 0" class="bannerImg" />
        <img src="../assets/Home/轮播2.png" v-if="index % 2 == 1" class="bannerImg" />
      </el-carousel-item>
    </el-carousel>

    <!-- 下面大框 开始-->
    <el-container class="bottom">

      <el-container class="left">
        <el-container class="constrained" @click.native="changeCategoryShow ()">
          <i :class="isCollapse? 'el-icon-s-unfold':'el-icon-s-fold'" />
        </el-container>
        <el-menu background-color="transparent"
                 text-color="#1C3263"
                 active-text-color="#55688C"
                 default-active="0"
                 :collapse-transition="false"
                 class="el-menu-vertical-demo"
                 :collapse="isCollapse"
                 :unique-opened="true">
          <el-submenu v-for="item in categoryList.data"
                      :key="item.id"
                      :index="item.id+''"
                      :disabled="(item.id == 2 || item.id == 3) ? true : false"
                      >
            <!-- 一级菜单 -->
            <template slot="title">
              <img src="../assets/Home/包.png" v-if="item.id == 1" class="categoryIcon"/>
              <img src="../assets/Home/饰品.png" v-if="item.id == 2" class="categoryIcon"/>
              <img src="../assets/Home/家具.png" v-if="item.id == 3" class="categoryIcon"/>
              <img src="../assets/Home/乐器.png" v-if="item.id == 4" class="categoryIcon"/>
              <img src="../assets/Home/旅行装备.png" v-if="item.id == 5" class="categoryIcon"/>
              <img src="../assets/Home/礼服.png" v-if="item.id == 6" class="categoryIcon"/>
              <span style="font-weight: bold;margin-right:50px;">{{item.categoryName}}</span>
            </template>

            <!-- 二级菜单 -->
            <el-menu-item v-for="subItem in item.subCategory"
                          :key="subItem.id"
                          :index="subItem.id+''"
            @click="chooseCategory(subItem)">
              <span style="margin-right:50px;">{{subItem.categoryName}}</span>

            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-container>

      <!-- 猜你喜欢大框 开始-->
      <el-container class="right">

        <!-- 猜你喜欢标题 -->
        <el-container class="guessYouLike-title-container">
          <span style="font-weight:bold;color: #1C3263;">{{ guessYouLikeTitle }}</span>
          <div class="a-short-line"></div>
        </el-container>

        <!-- 猜你喜欢内容1 开始-->
        <el-container class="guessYouLike-content-aline-container">
          <el-col :span="4" v-for="(o, index) in (currentItemCount - (currentPage - 1)*10) >= 5? 5:currentItemCount - (currentPage - 1)*10" :key="o" :index="index" :offset="(index % 5) > 0 ? 1:0">
            <el-card :body-style="{ padding: '0px'}">
              <img :src="currentItemUrls[index + (currentPage - 1)*10].imageAdd"
                   class="image">

              <div class="guessYouLike-content-cell">
                <span>{{currentItemUrls[index + (currentPage - 1)*10].itemName}}</span>
              </div>

              <div class="guessYouLike-content-cell">
                <span>RMB {{currentItemUrls[index + (currentPage - 1)*10].itemRentPrice}} /天</span>
              </div>

            </el-card>
          </el-col>
        </el-container>
        <!-- 猜你喜欢内容1 结束-->

        <!-- 行中间占位框 -->
        <div style="height:30px;width:100%;"></div>

        <!-- 猜你喜欢内容2 开始-->
        <el-container class="guessYouLike-content-aline-container" v-if="currentItemCount - (currentPage - 1)*10 - 5 > 0">
          <el-col :span="4"
                  v-for="(o, index) in (currentItemCount - (currentPage - 1)*10 - 5)>=5? 5:(currentItemCount - (currentPage - 1)*10 - 5)"
                  :key="o"
                  :index="index" :offset="(index % 5) > 0 ? 1:0">
            <el-card :body-style="{ padding: '0px' }">
              <img :src="currentItemUrls[index + 5 + (currentPage - 1)*10].imageAdd" class="image">

              <div class="guessYouLike-content-cell">
                <span>{{currentItemUrls[index + 5 + (currentPage - 1)*10].itemName}}</span>
              </div>
              <div class="guessYouLike-content-cell">
                <span>RMB {{currentItemUrls[index + 5 + (currentPage - 1)*10].itemRentPrice}} /天</span>
              </div>
            </el-card>
          </el-col>
        </el-container>

        <!-- 行中间占位框 -->
        <div style="height:30px;width:100%;"></div>
      </el-container>
      <!-- 猜你喜欢大框 结束-->
    </el-container>
    <!-- 下面大框 结束-->

    <!-- 分页符-->
    <el-pagination
      layout="prev, pager, next"
      :total="currentItemCount"
      :current-page.sync="currentPage"
    @current-change="changePage(currentPage)">
    </el-pagination>

    <!-- 最下方占位框 -->
    <div style="height:30px;"></div>
  </div>
</template>

<script>

// @ is an alias to /src
export default {
  name: 'home',
  created () {
    this.initDefaultUrls()
  },
  data () {
    return {
      input1: '',
      cate_11_urls: [],
      cate_41_urls: [
        {
          imageAdd: require('../assets/Home/乐器/3 EUMASTER-F CHRISTINA意大利当代制琴大师原装进口音乐厅级小提琴200000.png'),
          itemName: 'EUMASTER-F CHRISTINA意大利当代制琴大师原装进口音乐厅级小提琴',
          itemRentPrice: '2000.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/4 上海敦煌牌541:KK酸枝木琵琶骆驼骨相轸演奏琵琶上海民族乐器一厂 14109.png'),
          itemName: '上海敦煌牌541:KK酸枝木琵琶骆驼骨相轸演奏琵琶上海民族乐器一厂',
          itemRentPrice: '141.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/10 初始化乐器 芬达 Fender 美超 Ultra STRAT 011-8010:8022 电吉他 16300.jpg'),
          itemName: '初始化乐器 芬达 Fender 美超 Ultra STRAT 011-8010:8022 电吉他',
          itemRentPrice: '163.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/5 古筝敦煌牌敦煌古筝698T天真元韵 牡丹:书法【敦煌乐器旗舰店】11051.png'),
          itemName: '古筝敦煌牌敦煌古筝698T天真元韵 牡丹:书法[敦煌乐器旗舰店]',
          itemRentPrice: '110.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/1 德国 ROFFEE:罗菲 进口小号乐器降b调红铜小号乐器乐团专业演奏 17950.png'),
          itemName: '德国 ROFFEE:罗菲 进口小号乐器降b调红铜小号乐器乐团专业演奏',
          itemRentPrice: '180.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/2 德国ROFFEE:罗菲高音进口金铜高音降b分体直管萨克斯风乐器专业款 21960.png'),
          itemName: '德国ROFFEE:罗菲高音进口金铜高音降b分体直管萨克斯风乐器专业款',
          itemRentPrice: '220.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/9 木海琵琶 红酸枝老料琵琶乐器烫蜡抛光琵琶专业演奏考级琵琶 13000.png'),
          itemName: '木海琵琶 红酸枝老料琵琶乐器烫蜡抛光琵琶专业演奏考级琵琶',
          itemRentPrice: '130.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/6 橡树小院定制版红木酸枝独奏级中阮专业演奏高考店长推荐乐器厂 12000.png'),
          itemName: '橡树小院定制版红木酸枝独奏级中阮专业演奏高考店长推荐乐器厂',
          itemRentPrice: '120.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/8 红木浮雕龙402扬琴 北京星海乐器 洋 一级红铁木豆杨琴 专业演奏 11800.png'),
          itemName: '红木浮雕龙402扬琴 北京星海乐器 洋 一级红铁木豆杨琴 专业演奏',
          itemRentPrice: '118.00'
        },
        {
          imageAdd: require('../assets/Home/乐器/7 韩国原装PD水晶系列豪华款 架子鼓考级爵士鼓乐器酒吧演出鼓 21800.png'),
          itemName: '韩国原装PD水晶系列豪华款 架子鼓考级爵士鼓乐器酒吧演出鼓',
          itemRentPrice: '218.00'
        }
      ],
      cate_51_urls: [
        {
          imageAdd: require('../assets/Home/相机/Fujifilm:富士X-E4 高清数码 微单相机 xe3升级xe4 5699.jpg'),
          itemName: 'Fujifilm:富士X-E4 高清数码 微单相机 xe3升级xe4',
          itemRentPrice: '56.00'
        },
        {
          imageAdd: require('../assets/Home/相机/Nikon:尼康D3500KIT DX单反相机数码相机高清照相机旅游录像 4779.jpg'),
          itemName: 'Nikon:尼康D3500KIT DX单反相机数码相机高清照相机旅游录像',
          itemRentPrice: '48.00'
        },
        {
          imageAdd: require('../assets/Home/相机/Nikon:尼康Z6II新品全画幅微单相机4K 约2450万有效像素 Z6二代 13899.png'),
          itemName: 'Nikon:尼康Z6II新品全画幅微单相机4K 约2450万有效像素 Z6二代',
          itemRentPrice: '138.00'
        },
        {
          imageAdd: require('../assets/Home/相机/【专卖店】富士 XT4 机身五轴防抖旅游vlog高清4K数码文艺复古旗舰微单相机可选X-T4套机18-55 16-80 35mm F2 11800.jpg'),
          itemName: '富士 XT4 机身五轴防抖旅游vlog高清4K数码文艺复古旗舰微单相机可选X-T4套机18-55 16-80 35mm F2',
          itemRentPrice: '118.00'
        },
        {
          imageAdd: require('../assets/Home/相机/【专卖店】富士80mm F2.8 R LM OIS WR MACRO远摄定焦微距微单镜头XF80 2.8适合XT30 XT3 XT4 XS10 XT200相机 7790.jpg'),
          itemName: '富士80mm F2.8 R LM OIS WR MACRO远摄定焦微距微单镜头XF80 2.8适合XT30 XT3 XT4 XS10 XT200相',
          itemRentPrice: '78.00'
        },
        {
          imageAdd: require('../assets/Home/相机/【新品】Nikon:尼康Z 14-24mm f:2.8S尼克尔微单相机大光圈镜头16800.jpg'),
          itemName: 'Nikon:尼康Z 14-24mm f:2.8S尼克尔微单相机大光圈镜头',
          itemRentPrice: '168.00'
        },
        {
          imageAdd: require('../assets/Home/相机/富士 XS10 微单相机 X-S10机身五轴防抖旅游vlog高清4K数码相机 6999.jpg'),
          itemName: '富士 XS10 微单相机 X-S10机身五轴防抖旅游vlog高清4K数码相机',
          itemRentPrice: '70.00'
        },
        {
          imageAdd: require('../assets/Home/相机/富士XPRO3旗舰级旁轴微单相机 X-PRO3单机身 口袋小巧专业街拍视频复古相机 XPRO2升级版 12790.jpg'),
          itemName: '富士XPRO3旗舰级旁轴微单相机 X-PRO3单机身 口袋小巧专业街拍视频复古相机 XPRO2升级版',
          itemRentPrice: '128.00'
        },
        {
          imageAdd: require('../assets/Home/相机/尼康D3500 单反相机 学生 入门级 高清旅游 照相机 数码18-55防抖 3199.jpg'),
          itemName: '尼康D3500 单反相机 学生 入门级 高清旅游 照相机 数码18-55防抖',
          itemRentPrice: '32.00'
        },
        {
          imageAdd: require('../assets/Home/相机/带票行货】尼康Z5单机身24-50全画幅微单24-70f4套机高清旅游相机 9099.jpg'),
          itemName: '带票行货】尼康Z5单机身24-50全画幅微单24-70f4套机高清旅游相机',
          itemRentPrice: '91.00'
        }
      ],
      cate_61_urls: [
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Botanica 挂脖领拼色礼服连衣裙 14850.jpg'),
          itemName: 'Zimmermann Botanica 挂脖领拼色礼服连衣裙',
          itemRentPrice: '148.50'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Botanica 无袖蟒纹印花中长款连衣裙 6700.jpg'),
          itemName: 'Zimmermann Botanica 无袖蟒纹印花中长款连衣裙',
          itemRentPrice: '67.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Botanica 灯笼袖渐变立体印花迷你连衣裙 14650.jpg'),
          itemName: 'Zimmermann Botanica 灯笼袖渐变立体印花迷你连衣裙',
          itemRentPrice: '146.50'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Botanica 迷你立体蝴蝶长袖连衣裙 12650.jpg'),
          itemName: 'Zimmermann Botanica 迷你立体蝴蝶长袖连衣裙',
          itemRentPrice: ' 12650'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Botanica 金合欢立体印花蝴蝶装饰长裙15800.jpg'),
          itemName: 'Zimmermann Botanica 金合欢立体印花蝴蝶装饰长裙',
          itemRentPrice: '158.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Brighton 镂空贝壳花边迷你露腰连衣裙 8300.jpg'),
          itemName: 'Zimmermann Brighton 镂空贝壳花边迷你露腰连衣裙',
          itemRentPrice: '83.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Lovestruck 碎花泡泡袖亚麻连衣裙 4700.jpg'),
          itemName: 'Zimmermann Lovestruck 碎花泡泡袖亚麻连衣裙',
          itemRentPrice: '47.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Luminous 薰衣草彩绘印花中长款半身长裙 10700.jpg'),
          itemName: 'Zimmermann Luminous 薰衣草彩绘印花中长款半身长裙',
          itemRentPrice: '107.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann Mae 灯笼袖荷叶边迷你连衣裙 8700.jpg'),
          itemName: 'Zimmermann Mae 灯笼袖荷叶边迷你连衣裙',
          itemRentPrice: '87.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/正常/Zimmermann 迷你波点条纹连衣裙 4200.jpg'),
          itemName: 'Zimmermann 迷你波点条纹连衣裙',
          itemRentPrice: '42.00'
        }
      ],
      cate_62_urls: [
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/【婚纱定制】仙女教母FS 古典玩偶 lolita连衣裙 定制 13888.jpg'),
          itemName: '【婚纱定制】仙女教母FS 古典玩偶 lolita连衣裙 定制',
          itemRentPrice: '139.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/【婚纱定制】古典玩偶+伊丽莎白钻石星花裙(女王版)OP婚纱 定制 36688.jpg'),
          itemName: '【婚纱定制】古典玩偶+伊丽莎白钻石星花裙(女王版)OP婚纱 定制',
          itemRentPrice: '369.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/不妖 崋 中华风 Lolita连衣裙 婚纱定制 7654.jpg'),
          itemName: '不妖 崋 中华风 Lolita连衣裙 婚纱定制',
          itemRentPrice: '80.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/剧院爱丽丝茶会OP 古典玩偶 lolita连衣裙 定制 5888.jpg'),
          itemName: '剧院爱丽丝茶会OP 古典玩偶 lolita连衣裙 定制',
          itemRentPrice: '60.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/喵艾卿CatHighness原创华丽款开襟Lolita华鸟云岫现货小高腰OP 398.jpg'),
          itemName: '剧喵艾卿CatHighness原创华丽款开襟Lolita华鸟云岫现货小高腰OP',
          itemRentPrice: '30.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/国内现货 baby 中村十字 黑色jsk+斗篷+bnt 白色 掉落 不退换 6565.jpg'),
          itemName: '国内现货 baby 中村十字 黑色jsk+斗篷+bnt 白色',
          itemRentPrice: '66.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/国内现货 日本 BABY 天国少女樱茶OP含发梳 连衣裙 lolita正品 7500.png'),
          itemName: '日本 BABY 天国少女樱茶OP含发梳',
          itemRentPrice: '75.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/夜上海 金丝雀JSK 崋 中华风 lolita连衣裙 定制 7654.jpg'),
          itemName: '夜上海 金丝雀JSK 崋 中华风 lolita连衣裙 定制',
          itemRentPrice: '77.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/桃乐丝之夜原创猫塔罗印花Lolita洋装jsk高腰吊带连衣裙国牌现货 448.jpg'),
          itemName: '桃乐丝之夜原创猫塔罗印花Lolita洋装jsk高腰吊带连衣裙国牌现货',
          itemRentPrice: '40.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/白雪皇后 古典玩偶 Lolita连衣裙 定制 16888.jpg'),
          itemName: '白雪皇后 古典玩偶 Lolita连衣裙 定制',
          itemRentPrice: '169.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/结绿珍通贩版OP 崋 lolita连衣裙 现货 3188.jpg'),
          itemName: '结绿珍通贩版OP 崋 lolita连衣裙 现货',
          itemRentPrice: '32.00'
        },
        {
          imageAdd: require('../assets/Home/礼服/LOLITA/飞花令花嫁JSK FS 崋 lolita连衣裙 定制 9588.jpg'),
          itemName: '飞花令花嫁JSK FS 崋 lolita连衣裙 定制',
          itemRentPrice: '96.00'
        }
      ],
      isCollapse: true,

      guessYouLikeTitle: '猜你喜欢',
      guessYouLikeCategory: 11,
      currentItemCount: 0,
      currentItemUrls: [],
      categoryList: {
        data: [
          {
            id: 1,
            categoryName: '鞋品 / 箱包',
            subCategory: [
              {
                id: 11,
                categoryName: '女士挎包'
              }
            ]
          },
          {
            id: 2,
            categoryName: '饰品 / 配件',
            subCategory: []
          },
          {
            id: 3,
            categoryName: '家具 / 家电',
            subCategory: []
          },
          {
            id: 4,
            categoryName: '乐器 / 设备',
            subCategory: [
              {
                id: 41,
                categoryName: '乐器'
              }
            ]
          },
          {
            id: 5,
            categoryName: '旅行 / 装备',
            subCategory: [
              {
                id: 51,
                categoryName: '相机'
              }
            ]
          },
          {
            id: 6,
            categoryName: '礼服 / 戏服',
            subCategory: [
              {
                id: 61,
                categoryName: '礼服'
              },
              {
                id: 62,
                categoryName: 'Lolita'
              }
            ]
          }
        ]
      },
      currentPage: 1
    }
  },
  methods: {
    initDefaultUrls () {
      const tempData = [
        {
          imageAdd: require('../assets/Home/奢侈包包/2021新款凯莉女包法国原厂togo皮kelly25cm大象灰金扣斜挎手提包10800.jpg'),
          itemName: '2021新款凯莉女包法国原厂togo皮kelly25cm大象灰金扣斜挎手提包',
          itemRentPrice: '108.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/2021新款铂金女包米白色金扣雾面HCP尼罗鳄鱼皮birkin25cm手提包49800.jpg'),
          itemName: '2021新款铂金女包米白色金扣雾面HCP尼罗鳄鱼皮birkin25cm手提包',
          itemRentPrice: '498.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/BALENCIAGA巴黎世家 HOURGLASS Top Handle 小号 沙漏包 手袋14100.jpg'),
          itemName: 'BALENCIAGA巴黎世家 HOURGLASS Top Handle 小号 沙漏包 手袋',
          itemRentPrice: '141.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/BALENCIAGA巴黎世家 女士 Hourglass小号手提包沙漏包16500.jpg'),
          itemName: 'BALENCIAGA巴黎世家 女士 Hourglass小号手提包沙漏包',
          itemRentPrice: '165.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/BOTTEGA VENETA:葆蝶家2021新品THE CHAIN CASSETTE枕头包BV包女30600.jpg'),
          itemName: 'BOTTEGA VENETA:葆蝶家2021新品THE CHAIN CASSETTE枕头包BV包女',
          itemRentPrice: '306.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/BOTTEGA VENETA:葆蝶家2021新品女士THE TRIANGLE牛皮革BV手提包25800.jpg'),
          itemName: 'BOTTEGA VENETA:葆蝶家2021新品女士THE TRIANGLE牛皮革BV手提包',
          itemRentPrice: '258.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/BURBERRY 徽标图案水桶包 80368231  11500.jpg'),
          itemName: 'BURBERRY 徽标图案水桶包 80368231',
          itemRentPrice: '115.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/BURBERRY:博柏利女士棕色格子棉质手提包12719.jpg'),
          itemName: 'BURBERRY:博柏利女士棕色格子棉质手提包',
          itemRentPrice: '127.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/Bvlgari:宝格丽女士金属色牛皮拼接细链条单肩斜挎包21999.jpg'),
          itemName: 'Bvlgari:宝格丽女士金属色牛皮拼接细链条单肩斜挎包',
          itemRentPrice: '220.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/Bvlgari宝格丽女士牛皮斜挎包单肩包时尚女包包小包背包26000.jpg'),
          itemName: 'Bvlgari宝格丽女士牛皮斜挎包单肩包时尚女包包小包背包',
          itemRentPrice: '260.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/Celine 思林 女包复古皮质灰色翻盖简约单肩包斜挎包小方包37999.jpg'),
          itemName: 'Celine 思林 女包复古皮质灰色翻盖简约单肩包斜挎包小方包',
          itemRentPrice: '380.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/CELINE:赛琳 思琳 女士牛皮百搭超小号手提包笑脸包思林23150.jpg'),
          itemName: 'CELINE:赛琳 思琳 女士牛皮百搭超小号手提包笑脸包思林',
          itemRentPrice: '231.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/Dior:迪奥女士新款时尚格纹印花单肩斜挎小方包蒙田包女包35999.jpg'),
          itemName: 'Dior:迪奥女士新款时尚格纹印花单肩斜挎小方包蒙田包女包',
          itemRentPrice: '360.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/dunhill登喜路早春经典新生LOCK BAG锁包斜挎包13950.jpg'),
          itemName: 'dunhill登喜路早春经典新生LOCK BAG锁包斜挎包',
          itemRentPrice: '139.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/GIORGIO ARMANI:阿玛尼2021春夏女士水钻链条背提包19500.jpg'),
          itemName: 'GIORGIO ARMANI:阿玛尼2021春夏女士水钻链条背提包',
          itemRentPrice: '195.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/Hermes:爱马仕 康康包Constance翻盖H扣小方包单肩斜挎包女包26200.png'),
          itemName: 'Hermes:爱马仕 康康包Constance翻盖H扣小方包单肩斜挎包女包',
          itemRentPrice: '262.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/Hermes:爱马仕小方包猪鼻子包Roulis金扣翻盖单肩斜挎包女包45820.png'),
          itemName: 'Hermes:爱马仕小方包猪鼻子包Roulis金扣翻盖单肩斜挎包女包',
          itemRentPrice: '458.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/Mulberry:玛珀利新款女包Iris 系列中号手提包单肩包斜挎包HH6266  11800.jpg'),
          itemName: 'Mulberry:玛珀利新款女包Iris 系列中号手提包单肩包斜挎包HH6266',
          itemRentPrice: '118.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/TOD\'S官方正品刘诗诗同款2021春夏新品牛皮单肩包斜挎包腰包多用10600.jpg'),
          itemName: 'TOD\'S官方正品刘诗诗同款2021春夏新品牛皮单肩包斜挎包腰包多用',
          itemRentPrice: '106.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/TOD\'S官方正品情人节限定2021春夏新品女包女士Timeless小号腰包11900.jpg'),
          itemName: 'TOD\'S官方正品情人节限定2021春夏新品女包女士Timeless小号腰包',
          itemRentPrice: '119.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/VALENTINO GARAVANI:华伦天奴 Roman Stud 中号纳帕羊皮革大钉包21900.jpg'),
          itemName: 'VALENTINO GARAVANI:华伦天奴 Roman Stud 中号纳帕羊皮革大钉包',
          itemRentPrice: '219.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/YSL 圣罗兰 女包logo款风琴款单肩链条包斜挎包20009.jpg'),
          itemName: 'YSL 圣罗兰 女包logo款风琴款单肩链条包斜挎包',
          itemRentPrice: '200.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/YSL 圣罗兰 女包白色字母logo图案流苏翻盖单肩包链条包斜挎包20239.jpg'),
          itemName: 'YSL 圣罗兰 女包白色字母logo图案流苏翻盖单肩包链条包斜挎包',
          itemRentPrice: '202.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/YSL:圣罗兰 女士CASSANDRA粒面纹压印皮革搭扣手提包21300.jpg'),
          itemName: 'YSL:圣罗兰 女士CASSANDRA粒面纹压印皮革搭扣手提包',
          itemRentPrice: '213.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/YSL:圣罗兰 女士SOLFERINO中号BOX SAINT LAURENT皮革肩背包22200.jpg'),
          itemName: 'YSL:圣罗兰 女士SOLFERINO中号BOX SAINT LAURENT皮革肩背包',
          itemRentPrice: '222.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/YSL:圣罗兰 女士迷你黑色信封包字母标志压花真皮链条钱包10900.jpg'),
          itemName: 'YSL:圣罗兰 女士迷你黑色信封包字母标志压花真皮链条钱包',
          itemRentPrice: '109.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/元鱼2020新款鳄鱼皮女包真皮女包进口暹罗鳄肚皮女包正品手提包女14498.jpg'),
          itemName: '元鱼2020新款鳄鱼皮女包真皮女包进口暹罗鳄肚皮女包正品手提包女',
          itemRentPrice: '145.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/女包新款爱心圆饼包mini单肩斜挎链条包55980.png'),
          itemName: '女包新款爱心圆饼包mini单肩斜挎链条包',
          itemRentPrice: '559.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/英国诺大侠正品Dior迪奥Montaigne蒙田30斜挎包链条老花女包25180.jpg'),
          itemName: '英国诺大侠正品Dior迪奥Montaigne蒙田30斜挎包链条老花女包',
          itemRentPrice: '251.00'
        },
        {
          imageAdd: require('../assets/Home/奢侈包包/香港直邮FENDI 芬迪 女包 Kan I F系列金属F圆环单肩链条包斜挎包11509.jpg'),
          itemName: '香港直邮FENDI 芬迪 女包 Kan I F系列金属F圆环单肩链条包斜挎包',
          itemRentPrice: '115.00'
        }
      ]
      this.cate_11_urls = tempData
      this.currentItemUrls = tempData
      this.currentItemCount = tempData.length
    },
    chooseCategory (item) {
      this.guessYouLikeTitle = item.categoryName
      this.guessYouLikeCategory = item.id
      this.currentPage = 1
      switch (item.id) {
        case 11:
          this.currentItemUrls = this.cate_11_urls
          this.currentItemCount = this.cate_11_urls.length
          break
        case 41:
          this.currentItemUrls = this.cate_41_urls
          this.currentItemCount = this.cate_41_urls.length
          break
        case 51:
          this.currentItemUrls = this.cate_51_urls
          this.currentItemCount = this.cate_51_urls.length
          break
        case 61:
          this.currentItemUrls = this.cate_61_urls
          this.currentItemCount = this.cate_61_urls.length
          break
        case 62:
          this.currentItemUrls = this.cate_62_urls
          this.currentItemCount = this.cate_62_urls.length
          break
      }
    },
    changeCategoryShow () {
      this.isCollapse = !this.isCollapse
    },
    changePage (page) {
      this.currentPage = page
    }
  }
}
</script>

<style lang="less" scoped>
.head-container{
  background-color: #F2B50F;
  width: 100%;
  height: 90px;
  display: flex;
  justify-content: space-between;
}

.head-container.left{
  width: 20%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.head-container .middle{
  /*background-color: #8f8d85;*/
  width: 48%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
/deep/ .head-container .middle{
  .el-input__inner {
    border-radius: 20px;
  }
}
.head-container .right{
  width: 23%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.lunbo-container{
  width: 100%;
  height: 300px;
  background-color: aliceblue;
  display: flex;
  align-content: stretch;
}
.bannerImg{
  width: 100%;
  height: 300px;
}
.categoryIcon{
  height:23px;
  width:23px;
  margin-right:10px;
}
.guessYouLike-title-container{
  width: 95%;
  height: 100px;
  justify-content: center;
  align-items: center;
  display: flex;
  flex-direction: column;
}
.guessYouLike-title-container .a-short-line{
  width: 70px;
  height: 3px;
  background-color: #FFD768;
  margin-top: 2px;
}
.guessYouLike-content-container{
  width: 80%;
  justify-content: center;
  align-items: flex-start;
  display: flex;
  flex-direction: column;
}
.guessYouLike-content-aline-container{
  justify-content: flex-start;
  width: 100%;
}
.guessYouLike-content-cell{
  justify-content: flex-start;
  align-items: center;
  text-align: left;
  margin: 10px;
  font-size: x-small;
}
.bottom {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: flex-start;
}
/deep/ .bottom .left{
  margin-top: 60px;
  display: flex;
  height: 450px;
  flex-direction: row;
  justify-content: space-between;
  align-items: flex-start;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
  background-color: #FFD768;
  box-shadow: 1px 0.3px 5px rgb(182, 181, 181);
  .el-submenu .el-menu-item {
    min-width: 0;
  }
}

.el-menu-vertical-demo:not(.el-menu--collapse) {
  width: 85%;
  height: 450px;
  max-height: 450px;
  background-color: transparent;
}
.bottom .constrained{
  width: 20px;
  height: 450px;
  background-color: #FFD768;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
  box-shadow: 1px 0.3px 5px rgb(182, 181, 181);
}

.bottom .right{
  width: 90%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-left: 20px;
}
.image {
  width: 100%;
  display: block;
}

body > .el-container {
  margin-bottom: 40px;
}

.el-container:nth-child(5) .el-aside,
.el-container:nth-child(6) .el-aside {
  line-height: 260px;
}

.el-container:nth-child(7) .el-aside {
  line-height: 320px;
}
</style>
