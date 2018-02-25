<template>
    <div class="timepicker__container" :class="{'error': timeInvalid || timeRequiredInvalid}"  @keyup.27="isShown = false;">
        <label v-if="label" class="timepicker__label">{{ label }}</label>
        <div class="timepicker" v-on-clickaway="closeTimepickerPopup">
            <input class="timepicker__input" type="text" :placeholder="placeholder" v-model="updatedTime" @blur="validate" :required="required" @focus="closeTimepickerPopup()" />
            <div class="timepicker__icon" @click="toggleTimepickerPopup">
                <svg class="icon icon-time"><use xlink:href="#icon-time"></use></svg>
            </div>
            <div class="timepicker__popup" v-show="isShown">
                <span class="timepicker__popupCaret"></span>
                <div class="timepicker__inner">
                    <div class="timepicker__hoursContainer">
                        <span class="timepicker__arrowSelector" @click="incrementHour"><i class="timepicker__arrowUp"></i></span>
                        <span class="timepicker__numberHolder">{{ hour }}</span>
                        <span class="timepicker__arrowSelector" @click="decrementHour"><i class="timepicker__arrowDown"></i></span>
                    </div>
                    <div class="timepicker__colon">:</div>
                    <div class="timepicker__minutesContainer">
                        <span class="timepicker__arrowSelector" @click="incrementMinute"><i class="timepicker__arrowUp"></i></span>
                        <span class="timepicker__numberHolder">{{ minute }}</span>
                        <span class="timepicker__arrowSelector" @click="decrementMinute"><i class="timepicker__arrowDown"></i></span>
                    </div>
                    <div class="timepicker__ampmContainer">
                        <span class="ampm__selector" @click="toggleAMPM">{{ ampm }}</span>
                    </div>
                </div>
            </div>
        </div>
        <div v-if="timeInvalid" class="field-messages">Time entered is invalid.</div>
        <div v-if="timeRequiredInvalid" class="field-messages">This field is required.</div>
    </div>
</template>

<script>
    import moment from 'moment';
    import {
        mixin as clickaway
    } from 'vue-clickaway';
    export default {
        name: 'timepicker',
        mixins: [clickaway],
        props: {
            label: {
                type: String,
                required: false
            },
            placeholder: {
                type: String,
                default: 'hh:mm AM/PM'
            },
            required: {
                type: Boolean,
                default: false
            },
            time: {
                type: String
            }
        },
        data() {
            return {
                isShown: false,
                updatedTime: moment(this.time).format('h:mm A'),
                hour: moment(this.time).format('h'),
                minute: moment(this.time).format('mm'),
                ampm: moment(this.time).format('A'),
                timeInvalid: false,
                timeRequiredInvalid: false
            }
        },
        methods: {
            toggleTimepickerPopup() {
                this.isShown = !this.isShown;
            },
            closeTimepickerPopup() {
                this.isShown = false;
            },
            incrementHour() {
                this.hour > 11 ? this.hour = 1 : this.hour++;
                this.updateTime();
            },
            decrementHour() {
                this.hour < 2 ? this.hour = 12 : this.hour--;
                this.updateTime();
            },
            incrementMinute() {
                this.minute > 58 ? this.minute = 0 : this.minute++;
                (this.minute >= 0 && this.minute <= 9) ? this.minute = `0${this.minute}`: this.minute = this.minute;
                this.updateTime();
            },
            decrementMinute() {
                this.minute < 1 ? this.minute = 59 : this.minute--;
                (this.minute >= 0 && this.minute <= 9) ? this.minute = `0${this.minute}`: this.minute = this.minute;
                this.updateTime();
            },
            toggleAMPM() {
                this.ampm === 'AM' ? this.ampm = 'PM' : this.ampm = 'AM';
                this.updateTime();
            },
            updateTime() {
                this.timeInvalid = false;
                let newTime = new moment().set('hour', this.hour).set('minute', this.minute)
                this.updatedTime = `${moment(newTime).format('h:mm')} ${this.ampm}`;
                this.$emit('timepicker-updated', moment(newTime).toDate());
            },
            validate() {
    
                if (this.updatedTime.length) {
                    // Get time parts
                    let
                        x = this.updatedTime.split(':')[1],
                        hours = Number(this.updatedTime.split(':')[0]),
                        minutes = Number(x.split(' ')[0]),
                        ampm = String(x.split(' ')[1]);
    
                    // Validation rules for each part
                    let
                        hoursValid = (hours > 0) && (hours < 13) && (typeof hours === 'number'),
                        minutesValid = (minutes > -1) && (minutes < 60) && (typeof minutes === 'number') && (minutes.toString().length == 2),
                        ampmValid = (ampm.toUpperCase() === 'AM') || (ampm.toUpperCase() === 'PM');
    
                    if (ampmValid) this.ampm = ampm.toUpperCase();
    
                    // Display invalid styling
                    this.timeInvalid = !(hoursValid && minutesValid && ampmValid);
                    this.timeRequiredInvalid = false;
    
                    // Emit event for forms / pages to use. Data to emit along with event is up for discussion.
                    this.$emit('timepicker-invalid', this);
                } else {
                    this.timeRequiredInvalid = true;
    
                    // Emit event for forms / pages to use. Data to emit along with event is up for discussion.
                    this.$emit('timepicker-invalid', this);
                }
    
            }
        }
    }
</script>

<style lang="scss">
    $red: #c00;
    $darker-gray: #555;
    $dark-gray: #777;
    $light-gray: #ccc;
    $black: #111;
    $white: #fff;

    .timepicker__container.error {
        .timepicker__label {
            color: $red;
        }
        .timepicker__input {
            color: $red;
            border-color: $red;
        }
        .timepicker__icon {
            color: $red;
            border-color: $red;
        }
    }
    
    .timepicker {
        position: relative;
        display: flex;
        cursor: pointer;
        font-family: 'Avenir', 'Helvetica Nueue', sans-serif;
    }
    .timepicker__input {
        flex-grow: 1;
        padding: 10px;
        border: 1px solid $darker-gray;
        border-radius: 4px;
    }
    .timepicker__label {
        display: block;
        margin-bottom: 5px;
        font-size: 13px;
        font-weight: 600;
        line-height: 1.38;
        color: $darker-gray;
    }
    
    .timepicker > .timepicker__icon {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        outline: none;
        z-index: 2;
        display: block;
        width: 42px;
        line-height: 40px;
        border: 1px solid $dark-gray;
        border-radius: 0 4px 4px 0;
        background-color: $light-gray;
        text-align: center;
        &:hover {
            background-color: $light-gray * .9;
        }
    }
    .icon-time {
        width: 15px;
        height: 15px;
        fill: $darker-gray;
    }
    
    .timepicker__popup {
        position: absolute;
        border: 1px solid #ccc;
        border-radius: 5px;
        background: #fff;
        margin-top: 5px;
        z-index: 1000;
        width: 260px;
        height: 205px;
        top: 100%;
        right: 0;
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.175);
        cursor: pointer;
    }
    
    .timepicker__popupCaret {
        border-width: 7px;
        width: 0;
        height: 0;
        border-color: transparent;
        border-style: solid;
        position: absolute;
        top: -8px;
        right: 15px;
        border-top-width: 0;
        border-bottom-color: $black;
        z-index: 9;
        &:after {
            border-width: 9px;
            position: absolute;
            left: -9px;
            top: 0px;
            display: block;
            width: 0;
            height: 0;
            border-color: transparent;
            border-style: solid;
            content: "";
            border-top-width: 0;
            border-bottom-color: #fff;
            z-index: 10;
        }
    }
    
    .timepicker__inner {
        overflow: hidden;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        height: 100%;
        padding: 30px 25px;
        box-sizing: border-box;
    }
    
    .timepicker__hoursContainer,
    .timepicker__minutesContainer {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        text-align: center;
    }
    
    .timepicker__numberHolder {
        font-size: 16px;
        font-weight: bold;
        color: $black;
    }
    
    .timepicker__ampmContainer,
    .timepicker__colon {
        display: flex;
        flex-direction: column;
        justify-content: space-around;
    }
    
    .timepicker__ampmContainer {
        justify-content: space-around;
    }
    
    .ampm__selector {
        background-color: $dark-gray;
        padding: 6px 0;
        border-radius: 3px;
        color: $white;
        font-size: 16px;
        font-weight: bold;
        width: 35px;
        text-align: center;
        &:hover {
            background-color: $dark-gray * .9;
        }
    }
    
    .timepicker__colon {
        font-size: 16px;
        color: $black;
    }
    $arrowSize: 8px;
    $arrowColor: $dark-gray;
    .timepicker__arrowSelector {
        font-size: 16px;
        border-color: $dark-gray;
        &:hover {
            border-color: $dark-gray * .9;
        }
    }
    .timepicker__arrowUp {
        border-bottom: $arrowSize dashed $arrowColor;
        border-bottom: $arrowSize solid\9;
        border-right: $arrowSize solid transparent;
        border-left: $arrowSize solid transparent;
    }
    .timepicker__arrowDown {
        border-top: $arrowSize dashed $arrowColor;
        border-top: $arrowSize solid\9;
        border-right: $arrowSize solid transparent;
        border-left: $arrowSize solid transparent;
    }
</style>
