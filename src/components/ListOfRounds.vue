<template>
  <div class="list-rounds-container">
    <div v-for="(bid, index) in bidsArr" :key="index">
      <h3>
        {{ $t('Round') }}
        {{ index + 1 }}</h3>
      <div v-if="state ==='completed'" class="round-container round-container_completed">
        <div class="round-container__time-patricipant">
          <div class="round-container_time-completed">
            <img src="/static/images/watchInRound.png" alt="watch">
          </div>
          <div class="round-container_participant-completed">
            <div class="round-container_participant__order-number">
              {{ $t('Bidder') }}
              {{ index + 1 }}
            </div>
            <div class="round-container_participant-expended">
              MU-Moloka_komuuuuuuuuuuuuuuuuuuuuu
            </div>
          </div>
        </div>
        <div class="round-container_bid">
          <h4>{{ bid }} 
            {{ $t('UAH') }}
          </h4>
        </div>
      </div>
      <div v-else-if="state ==='active' || state == 'pendingOfRound'" class="round-container round-container_active">
        <div class="round-container__time-patricipant">
          <div class="round-container_time-active">
            <div class="round-container_time__watch-icon">
              <img src="/static/images/watchInRound.png" alt="watch">
            </div>
            <div class="round-container_time__watch">
              {{ pauseTime | moment("hh:mm:ss") }}
            </div>
          </div>
          <div class="round-container_participant_active">
            {{ $t('Bidder') }}
            {{ index + 1 }}
          </div>
        </div>
        <div class="round-container_bid">
          <h4>{{ bid }} 
            {{ $t('UAH') }}
          </h4>
        </div>
      </div>
    </div>
    <div v-if="state ==='active' || state == 'pendingOfRound'" id="active-round">
      <h3>
        {{ $t('Round') }}
        {{ bidsArr.length + 1 }}</h3>
      <div class="round-container round-container_active">
        <div class="round-container__time-patricipant round-container__time-patricipant-active">
          <div 
            class="round-container_time-active">
            <div 
              class="round-container_time-active__watch-icon">
              <radial-progress-bar
                v-if="state !== 'pendingOfRound' && (value > 0 && value < 100)"
                :value="value" :text="remainedTimeOfRound"
                :remained-time-of-round="remainedTimeOfRound"
                :duration-of-round="durationOfRound"
                min="0" max="100" />
            </div>
            <div class="round-container_time-active__watch">
              {{ currentTime | moment("hh:mm:ss") }}
            </div>
          </div>
          <div class="round-container_participant_active" />       
        </div>
        <div class="round-container_bid round-container_bid_active">
          <h4>
            {{ currentBid }} 
            {{ $t('UAH') }}
          </h4>
        </div>
      </div>
    </div>

    <div v-if="state ==='completed'" id="active-round" class="max-round-container">
      <h6><strong>
        {{ $t('Announcement') }}
      </strong></h6>
      <div class="round-container_completed round-container round-container_max">
        <div class="round-container__time-patricipant">
          <div class="round-container_time-completed round-container_time_max">
            <img src="/static/images/watchInRoundMax.png" alt="watch">
          </div>
          <div class="round-container_participant_completed">
            <div class="round-container_participant__order-number">
              {{ $t('Bidder') }}
              10
            </div>
            <div class="round-container_participant-expended">
              MU-Moloka_komuuuuuuuuuuuuuuuuuuuuu
            </div>
          </div>
        </div>
        <div class="round-container_bid round-container_bid_max">
          <h4 class="round-container_bid_max__bid-count">{{ currentBid }}
            {{ $t('UAH') }}
          </h4>
          <div class="round-container_bid_max-block">
            <div class="round-container_bid_max-block-count">
              MAX
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import RadialProgressBar from './RadialProgressBar.vue'
import AppListInitialOffers from './ListInitialOffers.vue'
export default {
  components: {
    RadialProgressBar,
    AppListInitialOffers
  },
  props: {
    bid: {
      type: Number,
      default: null
    },
    bidsArr: {
      type: Array,
      default: null
    },
    startBid: {
      type: Number,
      default: null
    },
    currentTime: {
      type: Number,
      default: null
    },
    currentBid: {
      type: Number,
      default: null
    },
    remainedTimeOfRound: {
      type: Number,
      default: null
    },
    durationOfRound: {
      type: Number,
      default: null
    },
    state: {
      type: String,
      default: null
    },
    pauseTime: {
      type: String,
      default: null
    },
  },
  data () {
    return {
      value: 0,
    }
  },
  watch: {
    remainedTimeOfRound() {
      this.value = (100 - (this.remainedTimeOfRound / this.durationOfRound * 100)).toFixed(2);
    },
  },
};
</script>

<style scoped>

.round-container{
    display: flex;
    justify-content: space-between;
    width: 100%;
    border: 1px solid #a1a1a1;
}
.round-container_active{
    height: 50px;
}

.round-container_completed{
    height: 60px;
}

.round-container_time-active{
    margin-left: -40px;
    width: 20%;
    display: flex;
    justify-content: space-between;
}

.round-container_time-completed{
    width: 10%;
    display:flex;
    justify-content: center;
}

.round-container__time-patricipant{
    width: 70%;
    display: flex;
    justify-content: space-around;
    align-items: center;
}

.round-container__time-patricipant-active{
    height: 95%;
    background-color: #e7f5ac;
    margin: auto;
    margin-left: 1px;
}

.round-container_bid{
    display: flex;
    justify-content: center;
    align-items: center;
    align-self: center;
    min-width: 30%;
    height: 90%;
    background-color: #e9e9e9;
    margin-right: 3px;
    font-family: Oswald;
}

.round-container_bid_active{
    height: 95%;
    background-color: #e7f5ac;
    margin: auto;
    margin-right: 1px;
}

.round-container_participant_active{
    width: 35%;
    text-align: center;
    font-family: 'Montserrat', sans-serif;
    font-size: 18px;
    font-weight: 800;
    line-height: 21.18px;
}

.max-round-container{
    margin-top: 30px;
}

.round-container_max{
    border: 1px solid #a0be22;
    margin-top: 15px;
}

.round-container_time_max{
    color: lightgreen;
}

.round-container_participant_completed{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    height: 100%;
    width: 70%;
}

.round-container_participant__order-number{
    display: flex;
}

.round-container_bid_max{
    background-color: #e7f5ac;
    justify-content: space-between;
}

.round-container_bid_max-block{
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #ffffff;
    height: 100%;
    width: 17px;
    font-size: initial;
}

.round-container_bid_max__bid-count{
    margin: 0 auto;
}

.round-container_bid_max-block-count{
    transform: rotate(-90deg);
    color: #000000;
    font-family: Montserrat;
}

</style>
