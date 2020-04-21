<template>
    <div class="select select-calendar">
        <div v-on:click="visibleCalendar=!visibleCalendar" class="select_btn" :class="{select_active: visibleCalendar}">
            <img src="../images/calendar-month.svg" alt="calendar">
            {{clickedDate}}/{{month + 1}}/{{year}}
            <div class="arrow_block"></div>
        </div>
        <div v-show="visibleCalendar" class="calendar">
            <div class="box">
                <button v-on:click="decrease" class="decrease">&#8592;</button>
                <h4>{{monthes[month]}}, {{year}}</h4>
                <button v-on:click="increase" class="increase">&#8594;</button>
            </div>
            <table>
                <thead>
                    <tr>
                        <td v-for="(d, indexDay) in day" :key='indexDay'>{{d}}</td>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(week, i) in calendar()" :key='i'>             
                        <td v-for="(day, index) in week" 
                        :key='index'
                        :class="{active: day.index === new Date().getDate() && new Date().getMonth() === month}"
                        @click="dayClicker($event)"> {{ day.index }} 
                        </td>
                    </tr> 
                </tbody>           
            </table>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            month: new Date().getMonth(),    
            year: new Date().getFullYear(), 
            dFirstMonth: '1',
            day:["Пн", "Вт","Ср","Чт","Пт","Сб", "Вс"],
            monthes:["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"],
            date: new Date(),
            dateNowMonth: new Date().getMonth(),
            visibleCalendar: false,
            clickedDate: new Date().getDate()
        }
    },
    methods:{
        calendar() {
            let days = [];
            let week = 0;
            days[week] = [];
            let dlast = new Date(this.year, this.month + 1, 0).getDate();
            for (let i = 1; i <= dlast; i++) {
                if (new Date(this.year, this.month, i).getDay() != this.dFirstMonth) {
                    let a = {index:i};
                    days[week].push(a);
                } else {
                    week++;
                    days[week] = [];
                    let a = {index:i};
                    days[week].push(a);
                }
            }
            if (days[0].length > 0) {
                for (let i = days[0].length; i < 7; i++) {
                    days[0].unshift('');
                }
            }
            this.dayChange;
            return days;
        },
        dayClicker(e) {
            this.clickedDate = e.target.innerHTML;
        },
        decrease() {
            this.month--;
            if (this.month < 0) {
                this.month = 12;
                this.month--;
                this.year--;
            }
        },
        increase: function(){
            this.month++;
            if (this.month > 11) {
                this.month = -1;
                this.month++;
                this.year++;
            }
        }
    }
}
</script>

<style lang="scss">
    .calendar {
        height: max-content;
        width: 200px;
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
        padding: 7px;
        td {
            text-align: center;
        }
        .active {
            background: #0288D1;
            color: #fff;
        }
        h4 {
            margin: 5px;
            text-align: center;
        }
        table {
            width: 100%;
            thead {
                td {
                    font-weight: bold;
                }
            }
            tbody {
                td {
                    cursor: pointer;
                }
            }
        }
        .box {
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
    }
    .select-calendar {
        img {
            margin-right: 10px;
        }
    }
</style>