<template>
  <section class="home">
    <aucto-node-navbar />
    <main class="aucto-owners">
      <h1 class="section-heading">
        <i class="fas fa-bolt icon"></i> Network Status:
        <span class="icon">Running</span>
      </h1>

      <section class="statistics">
        <section class="statistic">
          <h2 class="statistic__title">Total Dyzrupt Holders:</h2>
          <p class="statistic__stat">{{ auctoNodeOwnersCount }}</p>
        </section>
        <section class="statistic">
          <h2 class="statistic__title">Staking Pool Size:</h2>
          <p class="statistic__stat">{{ totalStakedAuctTokens | currency(' ')}}</p>
        </section>
        <section class="statistic">
          <h2 class="statistic__title">Reward ATH:</h2>
          <p class="statistic__stat">{{sharedRevenue}} Dyzrupt</p>
        </section>
        <section class="statistic">
          <h2 class="statistic__title">Next Payout:</h2>
          <p class="statistic__stat">{{getCurrentMonth() | uppercase}}</p>
        </section>
      </section>

      <h1 class="section-heading" style="margin-bottom: 1rem">
        <i class="fas fa-users icon"></i> Dyznode Explorer
      </h1>
      <section class="auct-node">
        <section class="header">
          <h2 class="header__title">Address</h2>
          <h2 class="header__title">Pool Size</h2>
          <h2 class="header__title" id="kyc-status">Staking status</h2>
        </section>
        <section
          v-if="!auctoNodeOwners.length"
          class="loading"
        >Dyznode is fetching owners. Please wait..</section>
        <section class="body" else>
          <section v-for="(auct, index) in auctoNodeOwners" :key="index" class="items">
            <section class="item">
              <p class="item__content">
                <a
                  :href="'https://wavesexplorer.com/address/' + auct.address"
                  target="_blank"
                >{{auct.address | truncate(30)}}</a>
              </p>
              <p class="item__content">{{ auct.quantity | currency(' ')}} Dyzrupt Token</p>
            </section>

            <p class="kyi-status">
              <span v-if="auct.status == 'Verified'">
                <i class="fa fa-certificate"></i>
              </span>
              {{ auct.status }}
            </p>
          </section>
        </section>
        <section class="mobile-auctonode" else>
          <section class="mobile-card" v-for="(auct, index) in auctoNodeOwners" :key="index">
            <section>
              <p class="address">
                <a
                  :href="'https://wavesexplorer.com/address/' + auct.address"
                  target="_blank"
                >{{auct.address | truncate(30)}}</a>
              </p>
              <p class="quantity">{{ auct.quantity | currency(' ')}} DYZ Token</p>

              <section class="mobile-auctonode__footer">
                <p class="kyc-status">
                  <span v-if="auct.status == 'Verified'">
                    <i class="fa fa-certificate"></i>
                  </span>
                  {{ auct.status }}
                </p>                
              </section>
            </section>
          </section>
        </section>
      </section>
      <footer>
        <section>
          <h3 class="footer">Dyznode &copy; {{ getCurrentYear() }}</h3>
          <br />
          <ul>
            <li>
              <a href="http://t.me/DYZchat">
                <i class="fas fa-futbol"></i> Need help?
              </a>
            </li>
            <li>
              <a href="https://www.dyzrupt.ltd/">
                <i class="fas fa-globe"></i> Our Website
              </a>
            </li>
            <li>
              <a
                href="https://www.youtube.com/channel/UCkBRapdM9TUlvdIIL0t4ULg/featured?view_as=public"
              >
                <i class="fas fa-question-circle"></i> How It Works
              </a>
            </li>
          </ul>
        </section>
        <section>
          <ul class="social">
            <li>
              <a href="http://t.me/DYZchat">
                <i class="fab fa-telegram"></i>
              </a>
            </li>
            <li>
              <a href="https://facebook.com/dyzrupt">
                <i class="fab fa-facebook-f"></i>
              </a>
            </li>
            <li>
              <a href="https://twitter.com/dyzrupt">
                <i class="fab fa-twitter"></i>
              </a>
            </li>
            <li>
              <a href="https://github.com/Dyzrupt-A1-dApp">
                <i class="fab fa-github"></i>
              </a>
            </li>
            <li>
              <a href="https://www.youtube.com/channel/UCkBRapdM9TUlvdIIL0t4ULg/featured?view_as=public">
                <i class="fab fa-youtube"></i>
              </a>
            </li>
          </ul>
        </section>
      </footer>
    </main>
  </section>
</template>

<script>
// @ is an alias to /src
import AuctoNodeNavbar from "@/components/navbar";
import { mapActions, mapState, mapGetters } from "vuex";
export default {
  name: "home",
  computed: {
    ...mapState([
      "isLoggedIn",
      "totalStakedAuctTokens",
      "auctoNodeOwners",
      "auctTokenHolders"
    ]),
    ...mapGetters(["auctoNodeOwnersCount"]),
    sharedRevenue() {
      return 279.135 + 51;
    }
  },
  methods: {
    ...mapActions(["login", "getAuctoNodeOwners"]),
    getCurrentMonth() {
      var months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];
      var now = new Date();
      return months[now.getMonth()];
    },
    getCurrentYear() {
      var now = new Date();
      return now.getFullYear();
    }
  },
  components: {
    AuctoNodeNavbar
  },
  created() {
    this.getAuctoNodeOwners();
  }
};
</script>

<style lang="scss" scoped>
$primary-color: #372145;
$secondary-color: darken($primary-color, 10%);
main {
  font-family: "Share Tech", sans-serif;
  padding: 1.25rem;
  background-color: $secondary-color;
  margin-top: 0;
  padding-top: 5vh;
  @media screen and (max-width: 767px) {
  }
}

.header {
  display: flex;
  justify-content: space-evenly;
  margin-bottom: 0.5rem !important;
  color: lighten(#e27b36, 25%);

  @media screen and (max-width: 767px) {
    & {
      display: none;
    }
  }
  .header__title {
    font-weight: normal;
    @media screen and (max-width: 767px) {
      font-size: 18px;
    }
  }
}

.body {
  @media screen and (max-width: 767px) {
    & {
      display: none;
    }
  }
  .items {
    display: flex;
    justify-content: space-around;
    margin: 0 auto;
  }

  .item {
    display: flex;
    justify-content: space-evenly;
    flex-direction: row;
    margin-bottom: 2rem !important;
    width: 60%;
    background-color: $primary-color;
    border-radius: 5rem;
    -webkit-box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);
    -moz-box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);
    box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);

    @media screen and (max-width: 767px) {
      justify-content: flex-end;
      width: 100%;
      overflow: auto;
    }
    a {
      color: rgba(248, 232, 11, 0.993);
      // color: lighten(#E27B36, 25%);
      text-decoration: none;
    }

    p.item__content {
      color: lighten(#e27b36, 25%);

      @media screen and (max-width: 767px) {
        font-size: 12px;
        margin-right: 0.5rem;
      }
    }
  }
}

.statistics {
  display: flex;
  justify-content: center;
  margin-bottom: 3rem;
  overflow: auto;

  @media screen and (max-width: 767px) {
    & {
      overflow: auto;
      width: 100%;
      justify-content: flex-start;
    }
  }
  .statistic {
    &:first-child {
      margin-left: 0.5rem;
    }

    width: 220px;
    height: 129px;
    margin-bottom: 30px;
    border-radius: 8px;
    border: none;
    background-image: linear-gradient(
      -135deg,
      $primary-color,
      $secondary-color
    );
    margin-right: 2rem;
    -webkit-box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);
    -moz-box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);
    box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);
    padding: 1.25rem;
    flex-shrink: 0;

    @media screen and (max-width: 767px) {
      width: 149px;
    }
    .statistic__title {
      font-weight: 500;
      color: lighten(#e27b36, 25%);
      margin-bottom: 25px;
      font-family: "Share Tech", sans-serif;
      @media screen and (max-width: 767px) {
        font-size: 24px;
      }
    }

    .statistic__stat {
      font-size: 30px;
      color: lighten(#e27b36, 25%);
      margin-bottom: 0;

      @media screen and (max-width: 767px) {
        font-size: 18px;
      }
    }
  }
}

.kyi-status,
.vote,
.loading {
  background-color: lighten(#e27b36, 25%);
  padding: 1rem 2rem;
  color: $secondary-color;
  border-radius: 4px;
  margin-left: -133px;
  margin-top: 0;
  align-self: flex-start;
  text-decoration: none;
  box-sizing: border-box;
}
@media screen and (max-width: 767px) {
  .kyi-status,
  .vote {
    display: none;
  }
}
.vote {
  transition: all 500ms;
  background-color: $primary-color;
  color: lighten(#e27b36, 25%);
  &:hover {
    color: $secondary-color;
    background-color: lighten(#e27b36, 25%);
  }
}

footer {
  padding: 3rem 3rem;

  @media screen and (max-width: 767px) {
    & {
      padding: 0;
      margin-bottom: 3rem;
    }
  }
  .footer {
    margin-bottom: 0 !important;
    // color: rgb(56, 53, 53);
    font-weight: normal;
  }
  ul {
    display: flex;
    list-style-type: none;
    padding: 0;
    li {
      margin-right: 1rem !important;
      a {
        text-decoration: none;
        color: lighten(#e27b36, 25%);
      }
      margin: 0;
    }
  }

  .social {
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;

    @media screen and (max-width: 767px) {
      & {
        justify-content: center;
        align-items: center;
      }
    }
  }
}

.section-heading {
  margin-left: 5rem;
  margin-bottom: 3rem !important;
  font-family: "Share Tech", sans-serif;
  color: #e27b36;
  @media screen and (max-width: 767px) {
    margin-left: 0;
    font-size: 20px;
  }
  .icon {
    color: rgba(248, 232, 11, 0.993);
  }
}
.loading {
  margin: 2rem auto;
  width: 20%;
  text-align: center;
  animation: bounce 900ms;
  animation-duration: 2s;
  animation-iteration-count: infinite;
  animation-timing-function: cubic-bezier(0.28, 0.84, 0.42, 1);
  transform-origin: bottom;

  @media screen and (max-width: 767px) {
    & {
      width: 50%;
    }
  }
}

@keyframes bounce {
  0% {
    transform: scale(1, 1) translateY(0);
  }
  10% {
    transform: scale(1.1, 0.9) translateY(0);
  }
  30% {
    transform: scale(0.9, 1.1) translateY(-100px);
  }
  50% {
    transform: scale(1.05, 0.95) translateY(0);
  }
  57% {
    transform: scale(1, 1) translateY(-7px);
  }
  64% {
    transform: scale(1, 1) translateY(0);
  }
  100% {
    transform: scale(1, 1) translateY(0);
  }
}

.mobile-auctonode {
  display: none;
}
@media screen and (max-width: 767px) {
  .mobile-auctonode {
    display: flex;
    flex-direction: column;
    .mobile-card {
      border-radius: 8px;
      border: none;
      background-image: linear-gradient(
        -135deg,
        $primary-color,
        $secondary-color
      );
      -webkit-box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);
      -moz-box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);
      box-shadow: -1px 1px 8px 0px rgba(0, 0, 0, 0.75);
      padding: 1.5rem 1rem;
      margin-bottom: 2rem;
      font-size: 18px;
      color: lighten(#e27b36, 25%);
      p.quantity {
        font-size: 24px;
      }
      .address {
        a {
          color: rgba(248, 232, 11, 0.993);
          text-decoration: none;
        }
      }
      .mobile-vote {
        background-color: lighten(#e27b36, 25%);
        padding: 1rem 2rem;
        color: $secondary-color;
        border-radius: 4px;
        text-decoration: none;
        text-align: center;
      }
    }
  }

  .mobile-auctonode__footer {
    display: flex;
    justify-content: space-between;
    width: 100%;
  }
}
</style>

