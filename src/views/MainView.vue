<template>
<div>
    <header class="header_container">
          <app-timer 
          @checkTimeOut="checkTimeOut"
          @getRemainedTimeofRound="getRemainedTimeofRound"
          @getCurrentTime="getCurrentTime"
          :date="dateOfAuction.date"
          :timeStatus="statusMessage[state].timeStatus"
          :state="state"
          >
            </app-timer>
      <app-status-info-label 
      :type="statusMessage[state].type"
      :textStatus="statusMessage[state].textStatus"
      :round="round"
      :state="state"
      :remainedTimeOfRound="remainedTimeOfRound"
      >
        </app-status-info-label>

    </header>
        <app-status-timer-line 
        v-if="state == 'active'"
        :durationOfRound="durationOfRound"
        :remainedTimeOfRound="remainedTimeOfRound"
        :state="state"
        >
          </app-status-timer-line>
    <main class="container container-main">
      <app-hong-audio-track
      v-if="state === 'active'"
      :hongTrack="hongTrack"
      >
        </app-hong-audio-track>
        <div class="container-main__tender-number">
          <div class="container-main__image-container">
            <img src="/static/images/numberOfTender_icon.png" alt="number-Of-tender">
          </div>
        {{tenderNumber}}
        </div>
        <div class="container-main__discribe-tender">
          <div class="container-main__discribe-tender_company-name">
            {{companyName}}
          </div>
          <ul class="container-main__discribe-tender_description-products">
            <li>
            {{descriptionOfProducts}}
            </li>
          </ul>
        </div>
        <app-start-rate 
        :startRate="startRate">
          </app-start-rate>

          <app-list-of-rounds-active v-if="state == 'active' || state == 'pendingOfRound'" 
          :roundArr="roundArr"
          :round="round"
          :rateArr="rateArr"
          :startRate="startRate"
          :currentRate="currentRate"
          :currentTime="currentTime"
          :remainedTimeOfRound="remainedTimeOfRound"
          >
          {{changeStateFromUrl}}
          </app-list-of-rounds-active>

           <app-list-of-rounds-completed v-if="state == 'completed'" 
          :roundArr="roundArr"
          :round="round"
          :rateArr="rateArr"
          :startRate="startRate"
          :currentRate="currentRate"
          :currentTime="currentTime"
          >
          </app-list-of-rounds-completed>
    </main>
    <footer  v-if="state !== 'completed'" class="footer-container">
          <h4
        v-if="state == 'pendingOfRound'">
        Очікується початок раунду
        </h4>

        <h4
        v-else-if="state == 'pendingOfAuction'"
        >Очікується початок аукціону
        </h4>

       <app-increasing-and-approval v-else-if="state == 'active'"
       @addNewRate="addNewRate"
       @calculateCurrentRate="calculateCurrentRate"
       @holdRoundTime="holdRoundTime"
       :startRate="startRate"
       :currentRate="currentRate"
       :rateArr="rateArr"
       >
         </app-increasing-and-approval>
         <app-test-select>
             </app-test-select>
    </footer>
</div>
</template>


<script>
export default {
  props: {
    id: {
      type: String,
    },
  },
  data(){
    return {
      state: 'active',
      hongTrack: 'https://upload.wikimedia.org/wikipedia/en/4/45/ACDC_-_Back_In_Black-sample.ogg',
      timeOut: false,
      currentTime: '',
      tenderNumber: 'UA-EA-2018-07-27-000020-B',
      companyName: 'AT "УКРГАЗВИДОБУВАННЯ :UA-EA-2018-07-27-000020-B aasdasdasas',
      descriptionOfProducts: 'Відпрацьовані акамуляторні батареї заправлені електролітом - 8.956 тонн',
      remainedTimeOfRound: 180,
      dateOfAuction: {
        date: Math.trunc(Date.parse('Wed Aug 28 2018 14:07:35 GMT+0200 (EET)') / 1000),
      },
      durationOfRound: 180,
      stoppingTimeOfRound: 30,
      startRate: 100.65,
      currentRate: 100.85,
      rateArr: [213213, 6546546, 2342343, 234234, 23423423, 234, 234, 234, 546],
      round: 1,
      roundArr: [],
      statusMessage: {
        active: {
          type: 'active',
          textStatus: 'Раунд  ',
          timeStatus: 'до закінчення раунду',
        },
        pendingOfAuction: {
          type: 'pending-of-auction',
          textStatus: 'Очікується',
          timeStatus: 'до початку аукціону',
        },

        pendingOfRound: {
          type: 'pending-of-round',
          textStatus: 'Очікується',
          timeStatus: 'до початку раунду',
        },
        completed: {
          type: 'completed',
          textStatus: 'Завершений',
          timeStatus: 'аукціон завершився',
        },

        canceled: {
          type: 'canceled',
          textStatus: 'Скасований',
          timeStatus: 'аукціон скасований',
        },
      },
    };
  },
  methods: {
    addNewRate(rate) {
      this.currentRate = rate * 1.05;
      this.rateArr.push(rate);
      this.roundArr.push(this.round);
      this.round++;
      this.dateOfAuction.date = this.currentTime + this.durationOfRound;
    },

    calculateCurrentRate(rate) {
      this.currentRate = rate * 1.05;
      this.dateOfAuction.date = this.currentTime + this.durationOfRound;
    },

    holdRoundTime() {
      this.dateOfAuction.date = this.currentTime + this.stoppingTimeOfRound;
      this.state = 'pendingOfRound';
    },

    checkTimeOut(res) {
      this.timeOut = res;
      if (res) {
        this.dateOfAuction.date = this.dateOfAuction.date + this.durationOfRound;
        this.state = 'active';
      }
    },
    getRemainedTimeofRound(remainedTime) {
      this.remainedTimeOfRound = remainedTime;
    },
    getCurrentTime(currentTime) {
      this.currentTime = currentTime;
    },
  },
    computed: {
    changeStateFromUrl() {
      this.state = this.id;
    },
  },

  mounted() {
    window.scrollTo(0, document.body.scrollHeight);
  },
};
</script>

<style>
  footer{
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    text-align: center;
  }
.header_container{
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 95px;
    background-color: #ffffff;
    box-shadow: 0 4px 9px 1px rgba(29, 29, 29, 0.09);
    display: flex;
    justify-content: space-between;
    align-items: center;
    }

.container-main{
  margin-bottom: 250px;
  margin-top: 100px;
}

.footer-container{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 95px;
    background-color: #e9e9e9;
    border-top: 3px solid #d9d9d9;
}

.container-main__tender-number{
    display: flex;
    color: #000000;
    font-family: Roboto;
    font-size: 20px;
    font-weight: 500;
    line-height: 26px;
    margin-bottom: 10px;
}

.container-main__image-container{
    margin-right: 15px;
}

</style>