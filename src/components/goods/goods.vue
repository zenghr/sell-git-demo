<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item clearfix border-1px">
              <div class="icon">
                <img width="57" height="57" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p v-if="food.description" class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="sellCount">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="newPrice"><sub>¥</sub>{{food.price}}</span>
                  <span v-if="food.oldPrice" class="oldPrice">¥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';

  const ERR_OK = 0;
  export default {
    name: 'goods',
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        goods: []
      };
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];

      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          this.$nextTick(() => {
            this._initScroll();
          });
          console.log(this.goods);
        }
      });
    },
    methods: {
      _initScroll() {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {});
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {});
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'

  .goods
    position: absolute
    top: 174px
    bottom: 48px
    width: 100%
    overflow: hidden
    display: flex
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background: #f3f5f7
      .menu-item
        height: 54px
        padding: 0 12px
        display: table
        .text
          display: table-cell
          vertical-align: middle
          width: 56px
          height: 54px
          font-size: 12px
          color: rgb(7, 17, 27)
          line-height: 14px
          border-1px(rgba(7, 17, 27, 0.1))
        .icon
          display: inline-block
          vertical-align: -2px
          width: 12px
          height: 12px
          margin-right: 2px
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
          /*background-image: url("decrease_1@2x.png")*/
          background-size: 12px 12px
          background-repeat: no-repeat
    .foods-wrapper
      flex: 1
      .title
        background: #f3f5f7
        border-left: 2px solid #d9dde1
        height: 26px
        line-height: 26px
        padding-left: 10px
        font-size: 12px
        color: rgb(147, 153, 159)
      .food-item
        margin: 0 18px
        padding: 18px 0
        border-1px(rgba(7, 17, 27, 0.1))
        /*font-size: 0*/
        display: flex /*弹性布局*/
        &:last-child
          border-none()
        .icon
          /*display: inline-block*/ /*水平布局*/
          flex: 0 0 57px /*弹性布局*/
          margin-right: 10px
          /*vertical-align: top*/ /*水平布局*/
          img
            border-radius: 2px
        .content
          /*display: inline-block*/ /*水平布局*/
          flex: 1 /*弹性布局*/
          font-size: 14px
          .name
            font-size: 14px
            color: rgb(7, 17, 27)
            line-height: 14px
            margin-top: 2px
          .desc, .extra
            font-size: 12px
            line-height: 12px
            color: rgb(147, 153, 159)
            margin: 8px 0
            .sellCount
              margin-right: 12px
          .price
            font-size: 0
            line-height: 14px
            font-weight: 700
            .newPrice
              font-size: 14px
              color: #f01414
              margin-right: 8px
              sub
                font-size: 10px
            .oldPrice
              font-size: 10px
              color: rgb(147, 153, 159)
              text-decoration: line-through
</style>
