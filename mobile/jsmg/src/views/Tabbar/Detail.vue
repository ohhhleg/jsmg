<template>
  <div class="top">
    <div class="product-show s-product-show">
      <div class="v2-3-swiper-container swiper-container-horizontal">
        <div class="swiper-wrapper">
          <div class="swiper-slide swiper-slide-active" style="width: 375px;">
            <img :src="srcs[0].src" />
          </div>
        </div>
        <div class="slider-source">
          <img src="../../assets/source_ziying.png" />
        </div>
      </div>

      <div class="s-product-info">
        <h1 v-html="srcs[0].title"></h1>

        <div class="p-price">
          <span class="price-current" v-html="'￥'+srcs[0].newpirce"></span>
          <span class="price-origin" v-html="srcs[0].oldpirce"></span>
          <van-tag color="black">9折</van-tag>
        </div>

        <div class="product_labels">
          <a>#篮球鞋</a>
          <a>#海外折扣精选</a>
        </div>
      </div>

      <div class="box_tips clearfix">
        <ul class="tips_ziying">
          <li><span>销量<s>10</s></span></li>
          <li><span>运费<s>0</s></span></li>
          <li><span>发货时间<s>3天</s></span></li>
        </ul>
      </div>

      <div class="box_tips s-discount-activity">
        <a>
          <van-tag plain type="primary" size="medium">满减</van-tag>
          <span class="s-activity-notice">开通【乐卡分期】,即赠50元购物券，再享12期免息！，去凑单 &gt;</span>
        </a>
      </div>
      <van-goods-action>
        <van-goods-action-icon icon="chat-o" text="客服" @click="onClickIcon" />
        <van-goods-action-icon icon="cart-o" text="购物车" @click="onClickIcon" />
        <van-goods-action-button text="加入购物车" @click="onClickButton" />
        <van-goods-action-button type="info" text="立即购买" @click="onClickButton" />
      </van-goods-action>
      <!-- 弹出层 -->
      <van-sku v-model="show" :sku="sku" :goods="goods" :goods-id="listid" :quota="quota"
        :hide-stock="sku.hide_stock" show-add-cart-btn reset-stepper-on-hide
        @buy-clicked="onBuyClicked" @add-cart="onAddCartClicked(listid)">
        
      </van-sku>
    </div>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        listid: this.$route.params.listid,
        srcs: [],
        stock: [],
        quota: 0,
        show: false,
        sku: {
          tree: [{
            k: "尺寸", // skuKeyName：规格类目名称
            v: [{
                id: '138',
                name: "38", // skuValueName：规格值名称
              },
              {
                id: '139',
                name: "39",
              },
              {
                id: '140',
                name: "40",
              },
              {
                id: '141',
                name: "41",
              },
              {
                id: '142',
                name: "42",
              },
              {
                id: '143',
                name: "43",
              },
              {
                id: '144',
                name: "44",
              },
              {
                id: '145',
                name: "45",
              },
            ],
            k_s: 's2'
          }],
          // 所有 sku 的组合列表，如下是：39、40、41、42
          list: [],
          price: '', // 默认价格（单位元）
          stock_num: 0 , // 商品总库存
          hide_stock: false, // 是否隐藏剩余库存
          // collection_id: 2261, // 无规格商品 skuId 取 collection_id，否则取所选 sku 组合对应的 id
          // none_sku: false, // 是否无规格商品
        },
        goods: {},
      };
    },
    async created() {
      //获取数据
      let srcs = await this.$axios.get(
        "http://localhost:3000/test"
      );
      // console.log(srcs.data.filter(item => item.listid==this.listid));
      this.srcs = srcs.data.filter(item => item.listid == this.listid);
      this.goods.title = this.srcs[0].title;
      this.goods.picture = this.srcs[0].src;
      this.sku.price = this.srcs[0].newpirce;

      let stock = await this.$axios.get(
        "http://localhost:3000/stock"
      );
      this.stock = stock.data.filter(item => item.listid == this.listid);

      // console.log(this.stock);

      this.stock.forEach((item, index) => {
        const list = {};
        list.price = this.sku.price;
        list.stock_num =Number(item.stock);
        list.goods_id = item.allid;
        list.s1 = 0;
        list.s3 = 0;
        list.s2 = item.numid;
        this.sku.list.push(list);
      });
    },

    methods: {
      onClickIcon() {},
      onClickButton() {
        this.show = !this.show;
      },
      onBuyClicked() {},
      onAddCartClicked(listid) {
        this.$router.push({
          name: "cart",
          params: {
            listid
          }
        })
      }
    },
  };

</script>
<style>
  .van-button--default {
    color: #1989fa;
  }

  .top {
    position: relative;
    top: 50px;
  }

  .product-show .v2-3-swiper-container {
    position: relative;
    overflow: hidden;
  }

  .swiper-wrapper {
    position: relative;
    width: 100%;
    height: 100%;
    z-index: 1;
    box-sizing: content-box;
  }

  .v2-3-swiper-container .swiper-wrapper .swiper-slide {
    width: 375px;
    display: flex;
    -webkit-box-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    align-items: center;
  }

  .v2-3-swiper-container .swiper-slide img {
    width: 100%;
    max-height: 100%;
  }

  .v2-3-swiper-container .slider-source {
    position: absolute;
    z-index: 1000;
    right: 3%;
    bottom: 3%;
  }

  .v2-3-swiper-container .slider-source img {
    width: 60px;
  }

  .product-show .s-product-info {
    background: #fff;
    padding: 15px;
    border-bottom: 1px solid rgba(227, 227, 227, 0.7);
  }

  .product-show .s-product-info h1 {
    font-size: 16px;
    font-weight: normal;
    color: #222;
    line-height: 28px;
  }

  .product-show .s-product-info .p-price {
    margin-top: 10px;
  }

  .product-show .s-product-info .p-price .price-current {
    color: #00aaea;
    font-size: 24px;
  }

  .product-show .s-product-info .p-price span.price-origin {
    font-size: 14px;
    color: #929292;
    margin-left: 5px;
  }

  .product-show .s-product-info .product_labels a {
    display: inline-block;
    margin-top: 5px;
    font-size: 13px;
    color: #929292;
    margin-right: 15px;
  }

  .product-show .box_tips {
    border-bottom: 1px solid rgba(227, 227, 227, 0.7);
    background-color: #fff;
  }

  .product-show .box_tips .tips_ziying li {
    width: 33%;
  }

  .product-show .box_tips li {
    margin: 15px 0;
    float: left;
    font-size: 12px;
    color: #222;
    text-align: center;
    border-right: 1px solid #eaeaea;
  }

  .product-show .box_tips li s {
    margin-left: 5px;
    color: #00aaea;
    text-decoration: none;
  }

  .product-show .s-discount-activity {
    margin-bottom: 10px;
    padding: 15px 10px;
    font-size: 12px;
    line-height: 2;
    position: relative;
  }

  .product-show .s-discount-activity a {
    display: block;
    width: 100%;
  }

  .product-show .s-discount-activity .s-activity-notice {
    margin-left: 24px;
  }

</style>
