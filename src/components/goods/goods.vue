<template>
  <div class="goods">
    <div class="menu-wrapper" v-el:menu-wrapper>
      <ul>
        <li class="menu-item" @click="selectMenu($index,$event)" v-for="item in goods"
            :class="{'current':currentIndex===$index}">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" v-el:foods-wrapper>
      <ul>
        <li v-for="item in goods" class="food-list food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border-1px" @click="selectFood(food,$event)">
              <div class="icon">
                <img width="57" height="57" :src="food.icon" alt="food picture">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span class="rating">好评{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="c-price">¥{{food.price}}</span><span class="o-price" v-show="food.olderPrice">¥{{food.oldPrice}}</span>
                </div>
                <div class="carcontrol-wrapper">
                  <carcontrol :food="food"></carcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart v-ref:shopcart :delivery-price="seller.deliveryPrice"
              :min-price="seller.minPrice" :select-foods="selectFoods"></shopcart>
  </div>

  <food :food="selectedFood" v-ref:food></food>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import shopcart from 'components/shopcart/shopcart';
  import carcontrol from 'components/carcontrol/carcontrol';
  import food from 'components/food/food';
  const ERR_OK = 0;
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      'shopcart': shopcart,
      'carcontrol': carcontrol,
      'food': food
    },
    data() {
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      };
    },
    created() {
      this.$http.get('/api/goods').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.goods = response.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calculateHeight();
          });
        }
      });
      this.classMap = ['decrease', 'discount', 'special', 'invoice',
        'guarantee'];
    },
    computed: {
      currentIndex() {
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i];
          let height2 = this.listHeight[i + 1];
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i;
          }
        }
        return 0;
      },
      selectFoods() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    },
    methods: {
      selectFood(food, event) {
          if (!event._constructed) {
            return;
          }
          this.selectedFood = food;
          this.$refs.food.show();
        },
      _initScroll() {
        this.menuScroll = new BScroll(this.$els.menuWrapper, {
          click: true
        });
        this.foodsScroll = new BScroll(this.$els.foodsWrapper, {
          probeType: 3,
          click: true
        });
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y));
        });
      },
      _calculateHeight() {
        let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i];
          height = height + item.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu(index, event) {
        if (!event._constructed) {
          return;
        }
        let foodList = this.$els.foodsWrapper.getElementsByClassName('food-list-hook');
        let el = foodList[index];
        this.foodsScroll.scrollToElement(el, 300);
      },
      _drop(target) {
        this.$refs.shopcart.drop(target);
      }
    },
    events: {
      'cart.add'(target) {
        this._drop(target);
      }
    }
  };
</script>

<style>
  .goods .foods-wrapper .food-item .content .carcontrol-wrapper {
    position: absolute;
    right: 0;
    bottom: 12px;
  }

  .border-1px:after {
    content: '';
    width: 100%;
    position: absolute;
    left: 0;
    bottom: 0;
    border-top: 1px solid rgba(7, 17, 27, 0.1);
  }

  .goods {
    display: flex;
    position: absolute;
    width: 100%;
    top: 174px;
    bottom: 46px;
    overflow: hidden;

  }

  .goods .menu-wrapper {
    width: 80px;
    flex: 0 0 80px;
    background-color: #f3f5f7;

  }

  .menu-wrapper .menu-item {
    display: table;
    height: 54px;
    width: 56px;
    line-height: 14px;
    padding: 0 12px;

  }

  .menu-wrapper .current {
    position: relative;
    z-index: 10;
    margin-top: -1px;
    background-color: #fff;
    font-weight: 700;

  }

  .menu-wrapper .menu-item .text {
    width: 56px;
    display: table-cell;
    vertical-align: middle;
    font-size: 12px;
    line-height: 14px;
    vertical-align: middle;

  }

  .menu-wrapper .menu-item .icon {
    display: inline-block;
    width: 12px;
    height: 12px;
    margin-right: 2px;
    vertical-align: top;
    background-size: 12px 12px;
    background-repeat: no-repeat;
  }

  .discount {
    background-image: url("discount_3@2x.png");
  }

  .special {
    background-image: url("special_3@2x.png");
  }

  .invoice {
    background-image: url("invoice_3@2x.png");
  }

  .guarantee {
    background-image: url("guarantee_3@2x.png");
  }

  .goods .foods-wrapper {
    flex: 1;
  }

  .foods-wrapper .title {
    padding-left: 14px;
    height: 26px;
    line-height: 26px;
    border-left: 2px solid #d9dde1;
    background-color: #f3f5f7;
    font-size: 12px;
    color: rgb(147, 153, 159);
  }

  .foods-wrapper .food-item {
    display: flex;
    margin: 18px;
    padding-bottom: 18px;
    position: relative;
  }

  .foods-wrapper .food-item:last-child:after {
    border-top: none;
  }

  .food-item .icon {
    flex: 0 0 57px;
    margin-right: 10px;
  }

  .food-item .content {
    flex: 1;
    font-size: 0;
  }

  .food-item .content .name {
    margin: 2px 0 8px 0;
    height: 14px;
    line-height: 14px;
    font-size: 14px;
    color: rgb(7, 17, 27);
  }

  .food-item .content .desc {
    margin-bottom: 8px;
    font-size: 10px;
    color: rgb(147, 153, 159);
    line-height: 12px;
  }

  .food-item .content .extra .count, .food-item .content .extra .rating {
    display: inline-block;
    font-size: 10px;
    color: rgb(147, 153, 159);
    line-height: 10px;
  }

  .food-item .content .extra .count {
    margin-right: 12px;
  }

  .food-item .content .price {
    font-weight: 700;
    line-height: 24px;
  }

  .food-item .content .price .c-price {
    margin-right: 8px;
    font-size: 14px;
    color: rgb(240, 20, 20);

  }

  .food-item .content .price .o-price {
    text-decoration: line-through;
    font-size: 10px;
    color: rgb(147, 153, 159);
  }
</style>
