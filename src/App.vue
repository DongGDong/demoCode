<template>
  <div id="app">
    <div class="content">
      <div class="contentMain">
        <div class="left">
          <div class="header clearfix">
            <div class="list">
              <ul>
                <li v-for="item in menuList" :key="item" @click="currentMenu = item" :class="{active:item === currentMenu}">{{item}}</li>
              </ul>
            </div>
            <div class="search">
              <input type="text" placeholder="search..." />
              <i class="iconfont icon-icon-test2" />
            </div>
          </div>
          <div class="cards">
            <h3 class="cross">
              <span class="title">Cards</span>
              <div class="viewAll">View all cards</div>
            </h3>
            <div class="cardsList">
              <ul class="clearfix">
                <li v-for="item in cardList" :key="item.card" class="card">
                  <p>Available balance</p>
                  <div class="balance">${{item.balance}}</div>
                  <div class="cardNumber">{{item | cardFormat}}</div>
                  <div class="cardBottom">
                    <div class="bank">{{item.bank}}</div>
                    <div class="change">
                      <span class="button" :class="{show:item.view}" @click="item.view = !item.view">

                      </span>
                    </div>
                  </div>
                </li>
                <li>
                  <div class="add">
                    <div class="icon"><i class="iconfont icon-plus"></i></div>
                    <div>Add New Card</div>
                  </div>
                </li>
              </ul>
            </div>
          </div>
          <div class="bottom">
            <div class="services">
              <h3 class="cross">
                <span class="title">Main Services</span>
                <div class="viewAll">View all</div>
              </h3>
              <div class="serviceList">
                <ul class="clearfix">
                  <li v-for="item in serviceList" :key="item.title">
                    <div class="serviceBox">
                      <i class="iconfont" :class="item.icon"></i>
                      <div class="title">{{item.title}}</div>
                    </div>
                  </li>
                </ul>
              </div>
            </div>
            <div class="right-bm">
              <div class="current">
                <h3 class="cross">
                  <span class="title">Current</span>
                  <div class="viewAll">View all</div>
                </h3>
                <div class="currentList">
                  <div v-for="item in currentList" :key="item.card" class="currentBox">
                    <div class="card">{{item.card}}</div>
                    <div class="center"></div>
                    <div class="money">${{item.money}}</div>
                  </div>
                </div>
              </div>
              <div class="saving">
                <h3 class="cross">
                  <span class="title">Savings</span>
                  <div class="viewAll">View all</div>
                </h3>
                <div class="savingMain">
                  <div class="chart" id="echart" style="height:200px;">
                  </div>
                  <div class="savingContent">
                    <div class="box">
                      <div class="name">Total</div>
                      <div class="value">${{savingData.total}}</div>
                    </div>
                    <div class="box">
                      <div class="name">This week</div>
                      <div class="value">${{savingData.week}}</div>
                    </div>
                    <div class="box">
                      <div class="name">This month</div>
                      <div class="value">${{savingData.month}}</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="right">
          <div class="header">
            <div class="notication">
              <i class="iconfont icon-notification"></i>
            </div>
            <div class="userInfo">
              <span>{{userInfo.name}}</span>
            </div>
          </div>
          <div class="charts" id="ring"></div>
          <div class="transactions">
            <h3 class="clearfix">
              <span>Transactions</span>
              <span class="date">Today</span>
            </h3>
            <div class="transContent">
              <div v-for="item in transcactionList" :key="item.title" class="transBox">
                <div class="box-left">
                  <i class="iconfont" :class="item.icon"></i>
                </div>
                <div class="box-center">{{item.title}}</div>
                <div class="box-right" :class="{bad: item.change < 0}">
                  {{item.change |  changeFormat}}
                </div>
              </div>
            </div>
            <div class="transferButton">
              <span>
                Transfer Money
              </span>
              <i class="iconfont icon-cc-arrow-right">

              </i>
            </div>
          </div>
          <div class="message">
            <i class="iconfont icon-message1"></i>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import echarts from 'echarts'
export default {
  name: 'App',
  data() {
    return {
      menuList: ['Dashboard', 'Services', 'History', 'Actions'],
      currentMenu: 'Dashboard',
      cardList: [
        {
          balance: 123,
          card: 1111111111111111,
          bank: 'Monzo',
          view: false,
        },
        {
          balance: 1213,
          card: 1111111111111167,
          bank: 'Monzo',
          view: false,
        },
        {
          balance: 123,
          card: 1111111111111131,
          bank: 'Monzo',
          view: false,
        },
        {
          balance: 1213,
          card: 1111111111111187,
          bank: 'Monzo',
          view: false,
        },
      ],
      serviceList: [
        {
          icon: 'icon-exchange4jiaohuan',
          title: 'Transactions',
        },

        {
          icon: 'icon-LC_icon_light_line',
          title: 'Utility',
        },
        {
          icon: 'icon-PurseOutline',
          title: 'Loans',
        },
        {
          icon: 'icon-wallet',
          title: 'Deposits',
        },
        {
          icon: 'icon-watch',
          title: 'Fast transfer',
        },
        {
          icon: 'icon-exchange3jiaohuan',
          title: 'Exchange',
        },
      ],
      currentList: [
        {
          card: '12345678910',
          money: '123123.12',
        },
        {
          card: '12345678910',
          money: '123123.12',
        },
        {
          card: '12345678910',
          money: '123123.12',
        },
      ],
      savingData: {
        total: 10000,
        week: 100,
        month: 5000,
        records: [],
      },
      myChart: null,
      userInfo: {
        name: 'Anastasia',
      },
      transcactionList: [
        {
          icon: 'icon-ball',
          title: 'Dribble Pro',
          change: 100,
        },
        {
          icon: 'icon-icon-test',
          title: 'Addidas',
          change: -200,
        },
        {
          icon: 'icon-icon-test1',
          title: 'wacom',
          change: 13000,
        },
      ],
    }
  },
  filters: {
    cardFormat({ card, view }) {
      card = card.toString()
      if (view) {
        let str = ''
        for (let i = 0; i < 4; i++) {
          str += `${i ? ' ' : ''}${card.slice(i * 4, (i + 1) * 4)}`
        }
        return str
      } else {
        return card.replace(/^\d{14}/, '**** **** **** **')
      }
    },
    changeFormat(val) {
      return val > 0 ? `+$${val}` : `-$${Math.abs(val)}`
    },
  },
  mounted() {
    this.myChart = echarts.init(document.getElementById('echart'))
    const options = {
      xAxis: {
        type: 'category',
        data: ['$10', '$20', '$30', '$40', '$50'],
        axisLine: {
          show: false,
        },
        axisTick: {
          // 刻度线
          show: false,
        },
        splitLine: {
          // 分割线
          show: false,
        },
      },
      yAxis: {
        type: 'value',
        axisLine: {
          show: false,
        },
        axisTick: {
          // 刻度线
          show: false,
        },
        splitLine: {
          // 分割线
          show: true,
          lineStyle: { color: '#cbcec6', type: 'dotted' },
        },
        max: 100,
        min: 0,
        axisLabel: {
          formatter(val) {
            return val + '%'
          },
        },
      },
      series: [
        {
          data: [3, 26, 40, 35, 60],
          type: 'line',
          smooth: true,
          itemStyle: {
            normal: {
              color: '#fff', // 折线点的颜色
              borderColor: '#42b2cc',
              borderWidth: 1,
              width: 2,
              lineStyle: {
                color: '#42b2cc',
                width: 4,
              },
            },
          },
        },
      ],
      grid: {
        left: '10%', // 组件离容器左侧的距离
        right: '10%',
        top: '10%',
        bottom: '10%',
      },
    }
    this.myChart.setOption(options)
    const ring = echarts.init(document.getElementById('ring'))
    const option = {
      backgroundColor: 'white',
      graphic: [
        {
          type: 'text',
          left: 'center',
          top: '40%',
          cursor: 'default',
          style: {
            text: '$5,349',
            textAlign: 'center',
            fill: '#1D1F25', // 文字的颜色
            height: 22,
            fontSize: 20,
            fontWeight: 'bold',
            // fontStyle: 'italic',
            fontFamily: 'TTTGBMedium',
          },
        },
        {
          type: 'group',
          left: 'center',
          top: '50%',
          children: [
            {
              type: 'rect',
              left: 'center',
              top: 'center',
              silent: true,
              shape: {
                width: 120,
                height: 30,
              },
              style: {
                fill: '#fff',
              },
            },
            {
              type: 'text',
              left: 'center',
              top: 'middle',
              silent: true,
              style: {
                fill: '#3A3C41', // 文字的颜色
                text: 'Balance',
                fontSize: 16,
                // fontWeight: 'bold',
                fontFamily: 'Microsoft YaHei',
                textAlign: 'center',
              },
            },
          ],
        },
      ],
      series: [
        {
          type: 'pie',
          clockwise: 'true',
          startAngle: '90',
          silent: true,
          radius: ['60%', '80%'],
          center: ['50%', '50%'],
          label: {
            show: false,
          },
          data: [
            {
              value: 112,
              name: '111',
              itemStyle: {
                color: '#0288D1',
              },
            },
            {
              value: 123,
              name: '222',
              itemStyle: {
                color: '#F0F2F5',
              },
            },
            {
              value: 123,
              name: '222',
              itemStyle: {
                color: '#F0F200',
              },
            },
          ],
        },
      ],
      grid: {
        left: '20%', // 组件离容器左侧的距离
        right: '20%',
        top: '20%',
        bottom: '20%',
      },
    }
    ring.setOption(option)
    addEventListener('resize', () => {
      this.myChart.resize()
      ring.resize()
    })
  },
}
</script>

<style lang="scss">
body {
  background: url('https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3711345961,2077937794&fm=26&gp=0.jpg')
    100% 100%;
  min-width: 800px;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  font-size: 14px;
  color: #2c3e50;
  width: 100%;
  .content {
    margin: 30px auto;
    width: calc(100% - 100px);
    padding: 10px;
    min-width: 600px;
    border-radius: 30px;
    background: #1b10104a;
    .contentMain {
      display: flex;
      border-radius: 30px;
      background: #fefefe;
      justify-content: space-between;
      .left {
        width: 75%;
        box-sizing: border-box;
        padding: 40px;
        > .header {
          height: 40px;
          line-height: 40px;
          .list {
            width: 60%;
            ul {
              margin: 0;
              li {
                float: left;
                list-style-type: none;
                font-size: 0.75px;
                margin-right: 1rem;
                color: #7d808c;
                &.active {
                  color: #000;
                }
              }
            }
          }
          .search {
            float: right;
            margin-top: 10px;
            width: 200px;
            height: 30px;
            line-height: 30px;
            background: #f8f8f8;
            border-radius: 10px;
            input,
            ::-webkit-input-placeholder {
              width: 85%;
              box-sizing: border-box;
              padding: 0 5px;
              background: #fff0;
              border: none;
              height: 100%;
              font-size: 10px;
              color: #cfd0d4;
              outline: none;
            }
            i {
              color: #c1c3c8;
              font-size: 10px;
            }
          }
        }
        .cross {
          display: flex;
          justify-content: space-between;
          font-size: 8px;
          font-weight: 400;
          padding: 5px 0;
          .title {
            color: #1d2b43;
            font-weight: 600;
          }
          .viewAll {
            color: #b1b3ba;
          }
        }
        .cards {
          .cardsList {
            ul li {
              width: 31%;
              float: left;
              height: 150px;
              margin-right: 2%;
              margin-bottom: 10px;
              box-sizing: border-box;
              padding: 10px;
              border-radius: 10px;
              background: #ff4d56;
              text-align: left;
              color: #de2c35;
              &:last-child {
                background: #f8f8f8;
                display: flex;
                justify-content: center;
                align-items: center;
                .add {
                  font-size: 13px;
                  color: #b2b3ba;
                  text-align: center;
                  .icon {
                    width: 24px;
                    height: 24px;
                    line-height: 24px;
                    margin-bottom: 10px;
                    border-radius: 12px;
                    margin: auto;
                    border: 1px dashed #e5e5e5;
                  }
                }
              }
              p {
                font-size: 10px;
                padding: 5px 0;
              }
              .balance {
                color: #ffff;
                font-size: 12px;
                margin-bottom: 20px;
              }
              .cardNumber {
                font-size: 14px;
              }
              .cardBottom {
                font-size: 14px;
                color: #fff;
                display: flex;
                justify-content: space-between;
                margin-top: 20px;
                .change .button {
                  width: 20px;
                  height: 20px;
                  margin-right: 20px;
                  display: inline-block;
                  position: relative;
                  background: #e30613;
                  border-radius: 10px;
                  &::after {
                    position: absolute;
                    content: '';
                    top: 0;
                    left: 15px;
                    width: 20px;
                    height: 20px;
                    display: inline-block;
                    position: relative;
                    background: #ffc24b;
                    border-radius: 10px;
                  }
                  &.show {
                    background: #fff;
                  }
                }
              }
            }
          }
        }
        .bottom {
          display: flex;
          .services {
            width: 30%;
            .serviceList {
              ul li {
                float: left;
                width: 50%;
                .serviceBox {
                  width: 80px;
                  height: 80px;
                  box-shadow: 1px 1px 5px #e2dbdb;
                  border-radius: 10px;
                  background: #fff;
                  margin-top: 10px;
                  i {
                    color: #ff6068;
                    font-size: 20px;
                    display: inline-block;
                    margin: 20px 0 10px;
                  }
                  .title {
                    font-size: 10px;
                    color: #646e7e;
                  }
                }
              }
            }
          }
          .right-bm {
            flex: 1;
            padding-left: 10px;
            .currentList {
              .currentBox {
                display: flex;
                padding: 10px;
                .card {
                  color: #bcbdc4;
                }
                .center {
                  flex: 1;
                  border-bottom: 1px dashed #efefef;
                  margin: 0 20px;
                }
                .money {
                  color: #1a2941;
                  font-weight: 600;
                  font-size: 14px;
                }
              }
            }
            .saving {
              .savingMain {
                display: flex;
                .chart {
                  flex: 1;
                }
                .savingContent {
                  width: 30%;
                  .box {
                    display: flex;
                    justify-content: space-between;
                    margin-top: 10px;
                    .name {
                      color: #b6b7be;
                    }
                    .value {
                      color: #2d3b51;
                      font-weight: 600;
                    }
                  }
                }
              }
            }
          }
        }
      }
      .right {
        width: 25%;
        box-sizing: border-box;
        padding: 40px 20px;
        position: relative;
        border-radius: 30px;
        background: #fff;
        box-shadow: -17px 0px 7px #f6f7f9;
        .header {
          display: flex;
          justify-content: space-between;
          .notication {
            color: #7d808c;
            i {
              font-size: 20px;
            }
          }
          .userInfo {
            height: 22px;
            line-height: 22px;
            > span {
              font-size: 12px;
              font-weight: 600;
            }
            img {
              border-radius: 22px;
              width: 22px;
              height: 22px;
            }
          }
        }
        .transactions {
          margin-top: 50px;
          h3 {
            text-align: left;
            color: #b8b9c0;
            .date {
              float: right;
            }
          }
          .transContent {
            .transBox {
              display: flex;
              margin-top: 30px;
              text-align: left;
              .box-left {
                width: 18%;
                i {
                  font-size: 20px;
                  color: #ff4d56;
                }
              }
              .box-center {
                width: 60%;
                font-size: 16px;
              }
              .box-right {
                width: 20%;
                color: #0e809a;
                font-size: 16px;
                &.bad {
                  color: #ff4d56;
                }
              }
            }
          }
          .transferButton {
            height: 40px;
            line-height: 40px;
            border-radius: 20px;
            margin-top: 30px;
            background: #ff4d56;
            color: #fff;
            text-align: left;
            padding: 0 20px;
            cursor: pointer;
            i {
              float: right;
            }
          }
        }
        #ring {
          height: 300px;
        }
        .message {
          width: 36px;
          height: 36px;
          line-height: 36px;
          background: #0e809a;
          border-radius: 18px;
          position: absolute;
          bottom: 20px;
          right: 20px;
          text-align: center;
          i {
            color: #fff;
          }
        }
      }
    }
  }
}
</style>
