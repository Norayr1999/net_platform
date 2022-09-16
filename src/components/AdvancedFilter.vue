<template>
<div>
    <button type="button" class="btn btn-success rounded-pill" data-bs-toggle="modal" data-bs-target="#exampleModalFullscreen">
        Advanced filters
    </button>
    <div class="modal fade" id="exampleModalFullscreen" tabindex="-1" aria-labelledby="exampleModalFullscreen" aria-hidden="true">
        <div class="modal-dialog modal-fullscreen">
            <div class="modal-content">
                <div class="modal-body p-0 d-flex">
                    <div class="text-uppercase flex-column d-flex bg-alabaster border-right">
                        <div
                        class="my-15 mx-5 pointer text-success font-weight-bold advanced-filter-tab text-start"
                        >OVERVIEW</div>
                    </div>
                    <div class="p-0 flex-column d-flex justify-content-between w-100 h-100">
                        <div class="d-flex flex-column justify-content-between overflow-auto h-100 p-4">
                            <div class="c-scrollbar flex-fill p-30 m-10">
                                <div>
                                    <div class="title d-flex"><span>LOCATION</span><hr/></div>
                                </div>
                                <div class="location">
                                    <input type="text" class="form-control rounded-pill" placeholder="Type to search" v-model="locationSearch">
                                    <div v-if="locationSearch" class="searchCountryList">
                                        <ul class="list-group">
                                            <li class="list-group-item country_list" v-for="(country,index) in filterCountries" :key="index" @click="addCountry(country)">
                                                {{country}}
                                            </li>
                                        </ul>
                                    </div>
                                </div>
                            </div>

                            <div class="c-scrollbar flex-fill p-30 m-10">
                                <div>
                                    <div class="title d-flex"><span>FOUNDED YEAR</span><hr/></div>
                                </div>
                                <div class="d-flex align-items-center">
                                    <div class="founder-input">
                                        <input type="text" class="form-control rounded-pill"
                                        v-model="from"
                                        placeholder="2010">
                                    </div>
                                    <div class="col-auto">
                                        to
                                    </div>
                                    <div class="founder-input">
                                        <input type="text" class="form-control rounded-pill" 
                                        v-model="to"
                                        placeholder="2022">
                                    </div>
                                </div>
                            </div>
                            <div class="c-scrollbar flex-fill p-30 m-10">
                                <div>
                                    <div class="title d-flex"><span>SIZE</span><hr/></div>
                                </div>
                                <div style="columns: auto 3">
                                    <div class="form-group" v-for="(size, index) in size_list" :key="size.id">
                                        <div class="checkbox checkbox-success checkbox_header d-flex align-items-center">
                                            <input class="input_checkbox" type="checkbox" :value="size.id" 
                                            v-model="selectedSize[index]"
                                            :id="'checkbox_'+size.id">
                                            <label class="checkbox_level mb-0" :for="'checkbox_'+size.id">{{size.text}}</label>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="c-scrollbar flex-fill p-30 m-10">
                                <div>
                                    <div class="title d-flex"><span>STAGE</span><hr/></div>
                                </div>
                                <div style="columns: auto 4">
                                    <div class="form-group" v-for="(stage, index) in stage_list" :key="index">
                                        <div class="checkbox checkbox-success checkbox_header d-flex align-items-center">
                                            <input class="input_checkbox" type="checkbox" :value="stage" 
                                            v-model="selectedStage[index]"
                                            :id="'checkbox_'+index">
                                            <label class="checkbox_level mb-0" :for="'checkbox_'+index">{{ stage.text }}</label>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="border-top" style="min-height: 80px;">
                                <ul class="list-inline text-start mb-0">
                                    <li class="list-inline-item">
                                        <div class="filter-wrapper pointer position-relative mt-2">
                                            <div class="filter d-flex align-items-center semi-rounded border-gray text-dark">
                                                <div class="text-center d-flex flex-wrap">
                                                    <div v-for="(item, idx) in finishList" :key="idx" class="mainFilter me-2 mb-2">
                                                        <span class="d-block finish-item">
                                                            {{ item.text }}
                                                        </span>
                                                        <div class="hide">
                                                            <div class="remove finish_item_icon"  @click="removeFilteredItem(item, idx)">
                                                                <span class="bg-success text-white d-flex justify-content-center align-items-center">
                                                                    <img src="https://app.netzeroinsights.com/images/icons/5.Cross_Icon.png"/>
                                                                </span> 
                                                            </div>
                                                            <div class="minimize finish_item_icon" @click="handleMinimize">
                                                                <span class="bg-success text-white d-flex justify-content-center align-items-center">
                                                                    <img src="https://app.netzeroinsights.com/images/icons/5.Minus_Icon.png"/>
                                                                </span> 
                                                            </div>
                                                        </div>  
                                                        
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-success rounded-pill">APPLY</button>
                    <button type="button" class="btn btn-outline-success rounded-pill">CLEAR ALL</button>
                    <button type="button" class="btn btn-link text-decoretion-none text-success" data-bs-dismiss="modal">CANCEL</button>
                    
                </div>
            </div>
        </div>
    </div>
</div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import { countries } from '../assets/list'

declare interface Player {
  id: string,
  text: string
}
export default defineComponent({
    data() {
        return {
            from: '' as string,
            to: '' as string,
            countries_list: countries as [],
            locationSearch: '' as string,
            selectedStage: [] as boolean[],
            selectedSize: [] as boolean[],
            selectedCountry: [] as Player[],
            size_list: [
                {id:'1_10', text: '1 - 10'},
                {id:'11_50', text: '11 - 50'},
                {id:'51_100', text: '51 - 100'},
                {id:'101_200', text: '101 - 200'},
                {id:'201_500', text: '201 - 500'},
                {id:'501_1000', text: '501 - 1000'},
                {id:'1001_5000', text: '1001 - 5000'},
                {id:'5001_10000', text: '5001 - 10000'},
                {id:'10001+', text: '10001+'}
            ],
            stage_list: [
                {id:'stage', text: 'Ideation'},
                {id:'stage', text: 'Early'},
                {id:'stage', text: 'Growth'},
                {id:'stage', text: 'Scaling'}
            ]
        }
    },
    computed: {
        filterCountries(): object {
            return this.countries_list.filter((el: string) => (el.toLowerCase()).includes(this.locationSearch.toLowerCase()))
        },
        filterFounded (): string {
            const item = (this.from && !this.to ? 'From '+ this.from : !this.from && this.to ? 'To ' : this.from+ ' - ') + this.to;

            if (item === ' - ') {
                return ''
            }

            return item
        },
        finishList (): object {
            const stages = this.filterStageAndSize(this.selectedStage, this.stage_list)                
            const size = this.filterStageAndSize(this.selectedSize, this.size_list)

            let data = [...Object.values(size), ...Object.values(stages)]

            if (this.filterFounded) {
                data.unshift({id: 'date', text: this.filterFounded })
            }
            data = [...this.selectedCountry, ...data]

            return data
        }
    },
    methods: {
        addCountry (country: string) {
            let obj = this.selectedCountry.filter((el, index) => el.text === country)
            if(!obj.length){
                this.selectedCountry.push({id:'country', text: country})
                this.locationSearch = '' 
            }
        },
        handleMinimize(event: { currentTarget: { parentElement: { previousSibling: { classList: { toggle: (arg0: string) => void; }; }; }; }; }) {
            event.currentTarget.parentElement.previousSibling.classList.toggle('textLine');
        },
        removeFilteredItem (item: { id: string; text: string }, index: number) {
            console.log(item)
            if(item.id === 'date') {
                this.from = '';
                this.to = '';
            }else if (item.id === 'stage') {
                const idx = this.stage_list.map((el, index) => el.text).indexOf(item.text)
                this.selectedStage[idx] = false
            }else if (item.id === 'country') {
                this.selectedCountry.splice(index, 1)
            }else {
                const idx = this.size_list.map((el, index) => el.text).indexOf(item.text)
                this.selectedSize[idx] = false
            }
        },
        filterStageAndSize (selectedItems: boolean[], items: string[] | object[]): object {
            return selectedItems.map((item, index) => {
                return item ? items[index] : null
            }).filter(item => item)
        }
    }
    });
</script>
<style scoped lang="scss">
$p100: 100%;
$rem09375: 0.9375rem;
$rem03125: 0.3125rem;

.hide {
    display: none;
}
.country_list:hover{
    cursor: pointer;
    background: lightgray
}
.searchCountryList {
    height: 250px;
    overflow-y: auto;
    border: 0.5px solid lightgray;
    border-radius: 5px
}
.location {
    width: 50%;
    position: relative;
}
    
.mainFilter:hover > .hide{
    display: block !important;
}

.mainFilter > span.textLine {
    text-decoration: line-through;
}

.checkbox_header {
    gap: 5px
} 
.mainFilter {
    position: relative;
}
.minimize {
    bottom: -0.5rem;
}
.finish_item_icon img {
    padding: 0.25rem;
    width: 1rem;
}
.finish_item_icon span{
    border-radius: 50%;
}
.finish_item_icon {
    cursor: pointer;
    font-size: .6875rem;
    position: absolute;
    border-radius: 50%;
    right: -0.5rem;
}
.remove {
    top: -0.5rem;
}
hr {
    height: 0.5px;
    width: 100%;
    margin-top: 0.7rem;
    border: 0
}
.input_checkbox {
    width: 20px;
    height: 20px;
    margin: 3px;
}
.modal-fullscreen {
    width: 100vw;
    max-width: none;
    height: $p100;
    margin: 0;

    .modal-body {
        & > div:first-child {
            flex: 20%;
        }

        .title > span {
            min-width: max-content !important;
        }

        .founder-input {
            max-width: 150px;
        }

        .finish-item {
            border: 1px solid grey;
            padding: 2px 8px;
            border-radius: 5px;
        }
    }
}
.text-start {
    text-align: start;
}
.height-100p {
    height: $p100;
}
.pt-25, .py-25 {
    padding-top: 1.5625rem!important;
}
.pl-15, .px-15 {
    padding-left: $rem09375 !important;
}

.pr-15, .px-15 {
    padding-right: $rem09375 !important;
}
.mb-15, .my-15 {
    margin-bottom: $rem09375 !important;
}

.mt-15, .my-15 {
    margin-top: $rem09375 !important;
}
.ml-5, .mx-5 {
    margin-left: $rem03125 !important;
}
.mr-5, .mx-5 {
    margin-right: $rem03125 !important;
}

@media screen and (max-width: 575px) {
    .modal-body {
        flex-direction: column;

        & > div:first-child {
            border-right: none !important;
            border-bottom: 1px solid #dee2e6!important;
            max-width: 100%;
            flex: none !important;
        }
    }
}
</style>