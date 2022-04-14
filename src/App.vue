<template>
  <div class="layout">
    <main :class="{ activated: VPNActive }">
      <svg id="map">
        <use xlink:href="#map_svg"></use>
      </svg>

      <header>
        <div class="menu">
          <span></span>
          <span></span>
          <span></span>
          <span></span>
        </div>

        <div class="premium">
          <svg>
            <use xlink:href="#crown_svg"></use>
          </svg>

          <span>Go Premium</span>
        </div>
      </header>

      <div class="locations" v-if="!VPNActive">
        <location v-for="location in locations" :location="location" :activeLocation="activeLocation" @change="changeLocation"></location>
      </div>

      <div class="timer_speed" v-else>
        <div class="timer">{{ countTime.hrs + ':' + countTime.min + ':' + countTime.sec }}</div>
        <div class="speed">
          <div class="download">
            <span class="chevron"></span>
            <p>12.4 <span>Mb/s</span></p>
          </div>

          <div class="upload">
            <span class="chevron"></span>
            <p>236.2 <span>Kb/s</span></p>
          </div>
        </div>
      </div>

      <div class="info">
        <div class="vpn_off" v-if="!VPNActive">
          <h1>Not Connected</h1>
          <div class="location_btn">
            <svg>
              <use xlink:href="#earth_svg"></use>
            </svg>
            <p>Location</p>
            <span class="chevron"></span>
          </div>
        </div>

        <div class="vpn_on" v-else>
          <p class="location_title">{{ locations[activeLocation].title }}</p>
          <h1>Connected</h1>
          <div class="ip">
            <img :src="require('@/assets/images/' + locations[activeLocation].url + '.png')" alt="">
            <span>{{ currentIP }}</span>
          </div>
        </div>
      </div>

      <div class="switch" @click="toggleVPN" :class="{ active: VPNActive }">
        <div class="button">
          <span class="indicator"></span>
          <p>{{ VPNActive === true ? 'STOP' : 'START' }}</p>
          <svg><use xlink:href="#power_svg"></use></svg>
        </div>

        <div class="chevrons top">
          <span class="small"></span>
          <span class="big"></span>
        </div>

        <div class="chevrons bottom">
          <span class="big"></span>
          <span class="small"></span>
        </div>
      </div>

      <div class="circles" :class="{ active: VPNActive }">
        <svg viewBox="0 0 100 100" v-for="index in 22">
          <circle cx="50" cy="50" :r="15 + index * 2" class="dotted" />
        </svg>
      </div>

      <svg-templates></svg-templates>
    </main>
  </div>
</template>

<script>
import Location from "@/components/Location";
import SvgTemplates from "@/components/ui/SvgTemplates";

export default {
  name: "App",
  components: {
    Location, SvgTemplates
  },
  data(){
    return {
      locations: [
        { id: 0, title: 'Wales', url: 'wales' },
        { id: 1, title: 'Canada', url: 'canada' },
        { id: 2, title: 'Estonia', url: 'estonia' },
        { id: 3, title: 'Ukraine', url: 'ukraine' },
        { id: 4, title: 'Norway', url: 'norway' },
        { id: 5, title: 'Germany', url: 'germany' },
        { id: 6, title: 'Scotland', url: 'scotland' },
      ],
      VPNActive: false,
      activeLocation: 3,
      currentIP: "182.198.0.1",
      countTime: { hrs: '00', min: '00', sec: '00' }
    }
  },
  methods: {
    toggleVPN(){
      this.VPNActive = !this.VPNActive;
      this.toggleTimer();
    },
    changeLocation(location){
      this.VPNActive = false
      this.toggleTimer();

      this.activeLocation = location.id

      this.currentIP = Math.floor(Math.random() * (255 - 10) + 10) + '.' +  Math.floor(Math.random() * (255 - 10) + 10) + '.' +  Math.floor(Math.random() * (255 - 10) + 10) + '.' +  Math.floor(Math.random() * (255 - 10) + 10)

      this.countTime = { hrs: '00', min: '00', sec: '00' }
    },
    toggleTimer(){

      if(this.timer){
        clearInterval(this.timer)
      }

      if(this.VPNActive){
        this.timer = setInterval(() => {

          let convert = (val) => (val < 10 ? '0' : '') + val;

          let sec = parseInt(this.countTime.sec);
          let min = parseInt(this.countTime.min);
          let hrs = parseInt(this.countTime.hrs);

          sec++

          if(sec > 59){
            sec = 0;
            min++;
          }

          if(min > 59){
            min = 0;
            hrs++
          }

          this.countTime = { hrs: convert(hrs), min: convert(min), sec: convert(sec) }

        }, 1000)
      }
    }
  }
}
</script>

<style lang="scss">

$black: rgb(17, 17, 17);
$darkblue: rgb(58, 56, 70);
$blue: rgb(85,84,99);
$lightgreen: rgb(108, 142, 129);
$green: rgb(69, 189, 93);
$white: rgb(235, 245, 244);

@import url('https://fonts.googleapis.com/css2?family=Anek+Tamil:wght@100;200;300;400;500;600;700;800&display=swap');

.layout{
  @include flex(row, center, center);
  height: 100vh;
}

main{
  @include flex(column, center, flex-start);
  position: relative;
  width: 100%;
  max-width: 380px;
  height: 98vh;
  min-height: 550px; max-height: 800px;
  background: $darkblue;
  border-radius: 60px;
  box-shadow: 15px 0 35px 20px rgba($darkblue, .5);
  padding-bottom: 100px;
  overflow: hidden;
  z-index: 1;

  &:before{
    content: '';
    position: absolute; bottom: 0; left: 0;
    width: 100%; height: 100%;
    background: linear-gradient(220deg, transparent 40%, $black 100%);
    z-index: 1;
  }

  #map{
    position: absolute; top: 17%; left: 5%;
    width: 90%; aspect-ratio: 1;
    fill: rgba($white, .03);
    z-index: 1;
  }

  header{
    @include flex(row, center, space-between);
    width: 100%;
    padding: 30px 30px 20px;

    .menu{
      width: 45px; height: 45px;
      display: grid;
      place-items: center;
      border: 1px solid rgba($white, .1);
      border-radius: 15px;

      span{
        width: 6px; height: 6px;
        grid-row: 1;
        grid-column: 1;
        background: $white;
        border-radius: 2px;

        &:nth-child(1){
          margin-top: -15px;
          opacity: .5;
        }

        &:nth-child(2){
          margin-right: -15px;
        }

        &:nth-child(4){
          margin-left: -14px;
        }

        &:nth-child(3){
          margin-bottom: -14px;
          opacity: .5;
        }
      }
    }

    .premium{
      @include flex(row, center, center);
      background: rgba(0,0,0,.1);
      padding: 10px 15px;
      border-radius: 25px;

      svg{
        width: 18px; height: 18px;
        margin-right: 10px;
      }

      span{
        font-size: 12px;
      }
    }
  }

  .locations{
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    width: 100%;
    padding: 15px 10px 0;
    height: 70px;
    position: relative;
    z-index: 2;
  }

  .timer_speed{
    @include flex(column, center, center);
    margin: 0 0 15px;

    .timer{
      font-size: 50px;
      font-weight: 500;
      margin: 0;
      line-height: 1.2;
    }

    .speed{
      @include flex(row, center, center);

      .download, .upload{
        @include flex(row, center, center);
        margin: 0 15px;

        .chevron{
          margin-right: 10px;
        }

        p{
          font-size: 12px;

          span{
            opacity: .6;
          }
        }
      }

      .download .chevron{
        @include chevron('top', $white, 8px);
        margin-bottom: -4px;
      }

      .upload .chevron{
        @include chevron('bottom', $white, 8px);
        margin-top: -4px;
      }
    }
  }

  .info{
    margin-top: auto;

    .vpn_on, .vpn_off{
      @include flex(column, center, center);
    }

    p{
      margin: 0;
    }

    h1{
      font-size: 30px;
      font-weight: 500;
      margin: 0;
      line-height: 1;
    }

    .location_title{
      opacity: .7;
      margin: 0 0 5px;
    }

    .location_btn{
      display: inline-flex;
      align-items: center;
      padding: 10px 20px 10px 10px;
      background: rgba($black, .3);
      border-radius: 20px;
      margin: 10px 0 0;

      svg{
        width: 18px; height: 18px;
        fill: $white;
      }

      p{
        margin: 0 10px;
        font-size: 14px;
      }

      .chevron{
        @include chevron('right', $white, 8px)
      }
    }

    .ip{
      @include flex(row, center, center);
      margin-top: 8px;

      img{
        width: 13px; height: 13px;
        margin-right: 5px;
      }

      span{
        font-size: 12px;
      }
    }
  }

  .switch{
    position: relative;
    width: 27%; height: 250px;
    margin: auto auto 0;
    background: rgb(18, 20, 22);
    padding: 5px;
    border-radius: 50px;
    box-shadow: 10px -10px 25px -5px rgba($white, .15), -10px 10px 25px rgba($black, .2);
    cursor: pointer;
    z-index: 3;
    @include no_select;

    .button{
      @include flex(column, center, center);
      position: absolute; top: 5px; left: 5px;
      width: calc(100% - 10px);
      box-shadow: 0 10px 15px -5px rgba($black, .3);
      background: $blue;
      border: 2px solid rgba($white, .04);
      padding: 35px 15px 15px;
      border-radius: 50px;
      z-index: 2;
      transition: .5s ease-in-out;

      .indicator{
        width: 15px; height: 3px;
        border-radius: 2px;
        background: rgba($black, .3);
        margin: 0 0 10px;
        transition: .2s ease-in-out;
      }

      p{
        margin: 0 0 15px;
      }

      svg{
        fill: $white;
        width: 50px; height: 50px;
        box-shadow: inset -2px 10px 15px rgba($black, .7), inset 3px -2px 5px rgba($white, .2);
        padding: 10px;
        border-radius: 50%;
        overflow: hidden;
      }
    }

    .chevrons{
      @include flex(column, center, center);
      position: absolute; width: 100%; left: 0;
      padding: 40% 0;
      z-index: 1;

      .small{
        opacity: .5;
      }
    }

    .chevrons.top{
      top: 0;

      .small{
        @include chevron('top', $white, 10px);
      }

      .big{
        @include chevron('top', $white, 14px);
        margin-top: -2px;
      }
    }

    .chevrons.bottom{
      bottom: 0;

      .small{
        @include chevron('bottom', $white, 10px);
      }

      .big{
        @include chevron('bottom', $white, 14px);
        margin-bottom: -2px;
      }
    }
  }

  .circles{
    display: grid;
    position: absolute; bottom: -35px; left: 0;
    width: 100%;
    transition: .3s ease-in-out;

    &:before{
      content: '';
      position: absolute; top: 0; left: 0;
      width: 100%; height: 70%;
      background: linear-gradient(180deg, $darkblue 60%, transparent 90%);
    }

    svg{
      grid-row: 1; grid-column: 1;

      circle {
        fill: transparent;
        stroke: $black;
        stroke-width: 1;
        width: 100%; height: 100%;
        stroke-dasharray: .1, 1.3;
        stroke-linecap: round;
        transition: .2s ease-in-out;
      }

      &:nth-child(3){
        circle {
          stroke: rgba($white, .7);
        }
      }
    }
  }

  .circles.active{
    svg{
      &:nth-child(1), &:nth-child(3), &:nth-child(5), &:nth-child(7){
        circle {
          stroke: $green;
        }
      }
      &:nth-child(3){
        circle {
          transition-delay: .05s;
        }
      }
      &:nth-child(5){
        circle {
          transition-delay: .1s;
        }
      }
      &:nth-child(7){
        circle {
          transition-delay: .15s;
        }
      }
    }
  }
}

main.activated{
  background: $lightgreen;

  .switch{
    .button{
      top: 100%;
      transform: translateY(calc(-100% - 5px));
      background: $lightgreen;

      .indicator{
        background: $green;
        box-shadow: 0 2px 7px 2px rgba($green, .5);
      }

      p{
        margin: 0 0 15px;
      }

      svg{
        fill: $white;
        width: 50px; height: 50px;
        box-shadow: inset -2px 10px 15px rgba($black, .7), inset 3px -2px 5px rgba($white, .2);
        padding: 10px;
        border-radius: 50%;
        overflow: hidden;
      }
    }
  }

  .circles{
    opacity: .5;

    &:before{
      background: linear-gradient(180deg, $lightgreen 60%, transparent 90%);
    }
  }
}


</style>