<template>
    <div class="date-picker">
        <div class="date-picker-header">
            <img
                 class="arrow arrow-left"
                 src="./arrow-right-24dp.svg"
                 @click="prevMonth"
            >
            <span class="selected-date">
                {{ `${monthNames[month]} ${year}` }}
            </span>
            <img
                 class="arrow"
                 src="./arrow-right-24dp.svg"
                 @click="nextMonth"
            >
        </div>

        <div class="date-picker-grid">
            <div
                    class="date-picker-day-cell text-small"
                    v-for="dayIndex of dayNamesIndexes"
                    :key="`day-${dayIndex}`"
            >
                <div class="color-grey-dark">
                    {{ dayNames[dayIndex] }}
                </div>
            </div>

            <div
                    class="date-picker-date-cell text-small"
                    v-for="({date, dateNumber}, i) of currentMonthDates"
                    :key="`date-${i}`"
                    :class="{'cell-muted': !isCurrentMonthDate(date)}"
            >
                <div v-if="dateNumber === 1" class="caption">
                    {{ getCaption(date) }}
                </div>
                <div>
                    {{ dateNumber }}
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="less" src="./style.less" scoped></style>

<script>
    const currentDate = new Date();

    export default {
        name: "WidgetCalendar",
        props: {
            monthNames: {
                type: Array,
                default() {
                    return [
                        'January',
                        'February',
                        'March',
                        'April',
                        'May',
                        'June',
                        'July',
                        'August',
                        'September',
                        'October',
                        'November',
                        'December',
                    ];
                },
            },
            dayNames: {
                type: Array,
                default() {
                    return ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'];
                },
                validator(value) {
                    return value.length === 7;
                }
            },
            dayOfWeek: {
                type: Number,
                default: 1,
                validator(value) {
                    return value >= 0 && value < 7;
                }
            }
        },
        data() {
            return {
                month: currentDate.getMonth(),
                year: currentDate.getFullYear(),
            }
        },
        computed: {
            dayNamesIndexes() {
                let daysIndexes = [];
                for (let i = this.dayOfWeek; i < 7 + this.dayOfWeek; ++i) {
                    daysIndexes.push(i >= 7 ? i - 7 : i);
                }
                return daysIndexes;
            },
            firstDayOfMonth() {
                const firstDayNumber = new Date(this.year, this.month, 1).getDay() - this.dayOfWeek;
                return firstDayNumber < 0 ? 7 + firstDayNumber : firstDayNumber;
            },
            lastDayOfMonth() {
                const lastDayNumber = new Date(this.year, this.month + 1, 0).getDay() - this.dayOfWeek;
                return lastDayNumber < 0 ? 7 + lastDayNumber : lastDayNumber;
            },
            daysInMonth() {
                return new Date(this.year, this.month + 1, 0).getDate();
            },
            currentMonthDates() {
                let dates = [...new Array(this.daysInMonth)].map(
                    (v, i) => new Date(this.year, this.month, i + 1)
                );
                return [ ...this.prevMonthDates, ...dates, ...this.nextMonthDates ].map( date => ({
                    date: date,
                    dateNumber: date.getDate()
                }))
            },
            prevMonthDates() {
                const dayNumber = this.firstDayOfMonth;
                let pmd = new Date(this.year, this.month, 0).getDate();
                return [...new Array(dayNumber)]
                    .map( (v, i) => new Date(this.year, this.month - 1, pmd - i) )
                    .reverse();
            },
            nextMonthDates() {
                const dayNumber = 6 - this.lastDayOfMonth;
                return [...new Array(dayNumber)]
                    .map( (v, i) => new Date(this.year, this.month + 1, i + 1) );
            },
        },
        methods: {
            prevMonth() {
                const monthIndex = this.month - 1;
                this.month = monthIndex < 0 ? 11 : monthIndex;
                this.year -= monthIndex < 0 ? 1 : 0;
            },
            nextMonth() {
                const monthIndex = this.month + 1;
                this.month = monthIndex > 11 ? 0 : monthIndex;
                this.year += monthIndex > 11 ? 1 : 0;
            },
            isCurrentMonthDate(date) {
                return this.month === date.getMonth();
            },
            getCaption(date) {
                return this.monthNames[date.getMonth()].substr(0, 3);
            }
        }
    }
</script>
