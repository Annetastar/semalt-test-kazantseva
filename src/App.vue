<template>
    <div id="app">
        <div class="sitemaps__container">
            <div class="sitemaps__container_header">
                <div>
                    <h1 class="title">Submitted sitemaps</h1>
                    <i class="icon"></i>
                </div>
                <div>
                    <button class="recrawl-btn">Recrawl sitemap</button>
                    <button class="remove-btn">Remove sitemap</button>
                </div>
            </div>
            <div class="sitemaps__container_filters">
                <h2 class="filters_title">Filters:</h2>
                <div class="sitemaps__container_filters_main">
                    <div class="select select_sitemaps">
                        <input class="select_btn" @click="visibleInput=!visibleInput" :class='{select_active: visibleInput}' v-model='inputValue' placeholder="URL or its part">
                        <ul v-show="visibleInput">
                            <li>
                                <input type="radio" id="sitemap" name="sitemaps" value="Sitemap contains" @click="radioTarget($event)" :class="eventValue === 'Sitemap contains' ? 'checked_radio' : ''">
                                <label for="sitemap">Sitemap contains</label>
                            </li>
                            <li>
                                <input type="radio" id="no-sitemap" name="sitemaps" value="Sitemap doesnt contain" @click="radioTarget($event)" :class="eventValue === 'Sitemap doesnt contain' ? 'checked_radio' : ''">
                                <label for="no-sitemap">Sitemap doesn't contain</label>
                            </li>
                            <li>
                                <input type="radio" id="match" name="sitemaps" value="Exact match" @click="radioTarget($event)" :class="eventValue === 'Exact match' ? 'checked_radio' : ''">
                                <label for="match">Exact match</label>
                            </li>
                            <li>
                                <button class="reset" @click="eventValue = '', inputValue = '', visibleInput = false">Reset</button>
                                <button class="apply" @click="visibleInput = false">Apply</button>
                            </li>
                        </ul>
                    </div>
                    <div class="select">
                        <div v-on:click="visibleTypes=!visibleTypes" class="select_btn" :class="{select_active: visibleTypes}">
                            All types
                            <div class="arrow_block"></div>
                        </div>
                        <ul v-show="visibleTypes">
                            <li>All types</li>
                        </ul>
                    </div>
                    <div class="select">
                        <div v-on:click="visibleSubmitted=!visibleSubmitted" class="select_btn" :class="{select_active: visibleSubmitted}">
                            Submitted
                            <div class="arrow_block"></div>
                        </div>
                        <ul v-show="visibleSubmitted">
                            <li>Submitted</li>
                        </ul>
                    </div>
                        <Calendar />
                    <div class="select">
                        <div v-on:click="visible=!visible" class="select_btn" :class="{select_active: visible}">
                            {{selectedSitemaps}}
                            <div class="arrow_block"></div>
                        </div>
                        <ul v-show="visible">
                            <li @click="selectedSitemaps = 'All sitemaps', visible = false">All sitemaps</li>
                            <li @click="selectedSitemaps = 'Success', visible = false">Success</li>
                            <li @click="selectedSitemaps = 'Couldnt fetch', visible = false">Couldn't fetch</li>
                            <li @click="selectedSitemaps = 'Errors', visible = false">Errors</li>
                        </ul>
                    </div>
                </div>
            </div>
            <div class="sitemaps__container_table">
                <Table :dataSitemaps='arrayItemsFiltered'/>
            </div>
        </div>
    </div>
</template>

<script>
import Table from './components/Table'
import Calendar from './components/Calendar'

export default {
  name: 'App',
  components: {
    Table,
    Calendar
  },
  data() {
    return {
      dataSitemaps: [],
      selectedSitemaps: 'All sitemaps',
      visible: false,
      visibleTypes: false,
      visibleSubmitted: false,
      visibleInput: false,
      radioActive: false,
      eventValue: '',
      inputValue: ''
    }
  },
  created() {
    fetch("https://mediglobus.ru/api")
    .then(response => response.json())
    .then(data => this.dataSitemaps = data.result.sitemap); 
  },
  methods: {
      radioTarget(e) {
        this.eventValue = e.target.value;
      }
  },
  computed: {
      arrayItemsFiltered() {
          if(this.inputValue === '') {
          if(this.selectedSitemaps === 'All sitemaps') {
              return this.dataSitemaps;
          } else if (this.selectedSitemaps === 'Success') {
              return this.dataSitemaps.filter(item => item.errors === 0);
          } else if (this.selectedSitemaps === 'Errors') {
              return this.dataSitemaps.filter(item => item.errors !== 0);
          }
          return this.dataSitemaps;
      } else if (this.inputValue !== '' && this.eventValue === 'Sitemap contains' || this.eventValue === 'Exact match') {
          return this.dataSitemaps.filter(item => {
              return item.path.indexOf(this.inputValue) !== -1;
          })
      } else if (this.inputValue !== '' && this.eventValue === 'Sitemap doesnt contain') {
          return this.dataSitemaps.filter(item => {
              return item.path.indexOf(this.inputValue) === 1;
          })
      }
      return this.dataSitemaps;
    }
  }
}
</script>

<style lang="scss">
    .body {
        font-family: 'Roboto', sans-serif;
    }
    .sitemaps__container {
        border: 1px solid #C2CFE0;
        border-radius: 5px;
        &_header {
        display: flex;
        justify-content: space-between;
        padding: 22px 24px;
        div:first-child {
            display: flex;
            align-items: center;
            .title {
                margin: 0 10px 0 0;
                font-weight: bold;
                font-size: 20px;
            }
            .icon {
                background-image: url('./images/icon.svg');
                background-repeat: no-repeat;
                display: block;
                width: 16px;
                height: 16px;
            }
        }
        div:last-child {
            display: flex;
            align-items: center;
            .recrawl-btn,
            .remove-btn {
                border: none;
                cursor: pointer;
                font-size: 14px;
            }
            .recrawl-btn {
                color: blue;
            }
            .remove-btn {
                color: red;
            }
        }
    }
    &_filters {
        height: 75px;
        background: #EAEDF5;
        padding: 10px;
        box-sizing: border-box;
        .select {
            &_btn {
                border: 1px solid #C2CFE0;
                box-sizing: border-box;
                border-radius: 5px;
                background: #fff;
                width: 100%;
                height: 30px;
                position: relative;
                display: flex;
                align-items: center;
                padding-left: 15px;
                outline: none;
                cursor: pointer;
                .arrow_block {
                    width: 30px;
                    height: 30px;
                    position: absolute;
                    right: 0;
                    border-left: 1px solid #C2CFE0;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    background-image: url('./images/arrow-up.svg');
                    background-repeat: no-repeat;
                    background-position: center;
                }
            }
            ul {
                width: 183px;
                height: max-content;
                background: #FFFFFF;
                border: 1px solid #C2CFE0;
                box-sizing: border-box;
                box-shadow: 0px 4px 8px rgba(199, 203, 210, 0.6);
                border-radius: 5px;
                margin: 0;
                padding: 0;
                position: absolute;
                list-style: none;
                z-index: 10;
                li {
                    height: 34px;
                    display: flex;
                    align-items: center;
                    cursor: pointer;
                    padding-left: 19px;
                    &:hover {
                        background: #F5F8FD;
                    }
                }
            }
            .select_active {
                border: 1px solid #0288D1;
                .arrow_block {
                    border-left: 1px solid #0288D1;
                    background-image: url('./images/arrow-up-active.svg')
                }
            }
        }
        .select_sitemaps ul {
            width: 275px;
            height: 179px;
            li {
                height: 45px;
                input {
                    position: relative;
                    visibility: hidden;
                    margin-right: 23px;
                    &:after {
                        content: '';
                        position: absolute;
                        background: #fff;
                        border: 1px solid #0288D1;
                        visibility: visible;
                        width: 18px;
                        height: 18px;
                        border-radius: 10px;
                        cursor: pointer;
                        top: -4px;
                        left: -4px;
                    }
                }
                .checked_radio {
                    &:before {
                        content: '';
                        width: 10px;
                        height: 10px;
                        background: #0288D1;
                        visibility: visible;
                        position: absolute;
                        z-index: 1;
                        border-radius: 10px;
                        cursor: pointer;
                        top: 1px;
                        left: 1px;
                    }
                }
                label {
                    width: 100%;
                    height: 100%;
                    display: flex;
                    align-items: center;
                    cursor: pointer;
                }
                &:last-child {
                    cursor: auto;
                    height: 38px;
                    display: flex;
                    justify-content: space-between;
                    padding-right: 19px;
                    .reset,
                    .apply {
                        border: none;
                        outline: none;
                        font-size: 14px;
                        cursor: pointer;
                    }
                    .reset {
                        color: #FB6868;
                    }
                    .apply {
                        color: #0288D1;
                    }
                    &:hover {
                        background: #fff;
                    }
                }
            }
        }
        .filters_title {
            margin: 0 0 7px 0;
            font-size: 14px;
            color: #37474F;
        }
        &_main {
            display: grid;
            grid-template-columns: repeat(5, 18%);
            grid-column-gap: 2%;
        }
    }
}
</style>