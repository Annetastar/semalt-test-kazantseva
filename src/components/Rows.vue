<template>
    <tr class="row" :class="{active_row: activeInput}">
        <td class="td-input">
            <input type="checkbox" id="check" class="checkbox" 
            @click='check($event)'
            :class="{active_input: activeInput}">
            <label for="check"></label>
        </td>
        <td class="path">
            <a :href='item.path'>{{'/' + item.path.split('/')[3]}}</a>
        </td>
        <td>{{ item.isSitemapsIndex ? 'Sitemap index' : '' }}</td>
        <td>{{findDate(item.lastSubmitted)}}</td>
        <td>{{findDate(item.lastCheck)}}</td>
        <td :class="item.errors === 0 ? 'succes_td' : 'error_td'">{{ (item.errors === 0 && item.warnings === 0) ? 'Success' :  item.errors + ' errors'}}</td>
        <td>{{item.urls}}</td>
        <td>
            <button class="recrewl__items-btn">Recrewl</button>
        </td>
        <td>
            <button class="remove__items-btn"></button>
        </td>
    </tr>
</template>

<script>

export default {
    props: ['item'],
    data() {
        return {
            activeInput: false,
            activeInputArray: []
        }
    },
    methods: {
        findDate(el) {
            let monthes = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
            let date = new Date(el);
            return `${monthes[date.getMonth()]} ${date.getDate()}, ${date.getFullYear()}`;
        },
        check(e) {
            if (e.target.checked === true) {
                this.activeInput = true;
                this.activeInputArray.push('1');
            } else if (e.target.checked === false) {
                this.activeInput = false;
                this.activeInputArray.pop('1');
            }
        }
    }
}
</script>

<style lang="scss">
    .row {
        height: 64px;
        border-bottom: 1px solid #F2F4FB;
        &:last-child {
            border-bottom: none;
        }
        td {
            text-align: center;
            &:nth-child(8) {
                width: 90px;
            }
            &:last-child {
                width: 90px;
            }
        }
        .td-input {
            width: 40px;
        }
        .checkbox {
            position: relative;
            visibility: hidden;
            &:after {
                content: '';
                visibility: visible;
                width: 17px;
                height: 17px;
                border: 1px solid #C2CFE0;
                display: block;
                border-radius: 3px;
                cursor: pointer;
                top: -4px;
                left: -3px;
                position: absolute;
            }
        }
        .active_input {
            &:before {
                content: url('../images/check.svg');
                visibility: visible;
                width: 18px;
                height: 18px;
                display: block;
                position: absolute;
                top: -3px;
                left: -2px;
                cursor: pointer;
            }
        }
        .path {
            text-align: left;
            a {
                color: #0288D1;
                text-decoration: none;
                position: relative;
                &:after {
                    content: url('../images/path.svg');
                    margin-left: 10px;
                    position: absolute;
                    top: 2px;
                }
            }
        }
        .recrewl__items-btn {
            height: 25px;
            width: 75px;
            font-size: 13px;
            border: 1px solid #0288D1;
            box-sizing: border-box;
            border-radius: 4px;
            color: #0288D1;
            outline: none;
            cursor: pointer;
            &:hover {
                background: #D2EFFF;
            }
        }
        .remove__items-btn {
            background-image: url('../images/backet.svg');
            background-repeat: no-repeat;
            height: 20px;
            width: 14px;
            border: none;
            cursor: pointer;
                &:hover {
                    background-image: url('../images/backet-active.svg')
                }
        }
        .succes_td {
            color: #2AC9A1;
        }
        .error_td {
            color: #FB6868;
        }
    }
    .active_row {
        background: #E7F6FF;
    }
</style>