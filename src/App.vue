<template >
    <div class="container mx-auto flex flex-col items-center bg-gray-100 p-4">
    <div class="container">
      <div class="w-full my-4"></div>
<!-- тут я ловлю событие add-block-item  -->
      <Add-Block @add-block-item="add" /> 
<!-- если в айтемс блок есть элементы то выводит поосочки сверху и с низу -->
        <hr v-if='blockItems.length' class="w-full border-t border-gray-600 my-4" />
        <div>
          <button 
        v-if="page>1"
        @click="page=page-1"
        class="my-4 mx-2 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
>Назад</button>
<button 
class="my-4 mx-2 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
@click="page=page+1"
v-if="hasNextPage"
>Вперед</button>
<div>Фильтр: 
<input v-model="filterData"
placeholder="DOGE"
style="border: 1px solid #718096; padding:5px; border-radius: 30px;"> </div>
<!-- связал поле инпут и переменную filterData -->
<hr v-if='blockItems.length' class="w-full border-t border-gray-600 my-4" />
</div>
        <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
          <!-- продублировал див столько раз сколько элементов в массиве blockItems -->
            <!-- v-bind:key="el" можно заменить на :key="el" -->
            <!--         @click="sell=el" при клике на блок в sell попадает объект  -->
            <!-- :class="sell===el? 'border-4':'' " заменили на :class="{'border-4': sell===el
}"   означает одно и тоже. т.е добавление класса  eсли элемент выбран в sell== el-->

          <div 

          v-for=" el in separationBlockItems"
            :key="el.name"
                  @click="select(el)"

            :class="{
  'border-2': sell===el
}"

            class="bg-white overflow-hidden shadow rounded-lg border-purple-800 border-solid  cursor-pointer"
          >
            <div class="px-4 py-5 sm:p-6 text-center">
              <dt class="text-sm font-medium text-gray-500 truncate">
                <!-- динамически выводим имя и стоимость в блоки -->
                {{el.name}}
              </dt>
              <dd class="mt-1 text-3xl font-semibold text-gray-900">
                                {{el.price}}

              </dd>
            </div>
            <div class="w-full border-t border-gray-200"></div>
            <!-- высываю функцию для удаления элементов с аргументом el -->
                    <!-- @click.stop="remove(el)"  прекратил всплытие события .stop -->

            <button
        @click.stop="remove(el)" 
                      class="flex items-center justify-center font-medium w-full bg-gray-100 px-4 py-4 sm:px-6 text-md text-gray-500 hover:text-gray-600 hover:bg-gray-200 hover:opacity-20 transition-all focus:outline-none"
            >
              <svg
                class="h-5 w-5"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 20 20"
                fill="#718096"
                aria-hidden="true"
              >
                <path
                  fill-rule="evenodd"
                  d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                  clip-rule="evenodd"
                ></path></svg
              >Удалить
            </button>
          </div>
        </dl>
        <hr v-if='blockItems.length' class="w-full border-t border-gray-600 my-4" />
        <!-- sell если есть то отображается -->
      <section v-if='sell' class="relative" > 
        <h3 class="text-lg leading-6 font-medium text-gray-900 my-8">
          <!-- добаление динамически имени при клике не блок -->
          {{sell.name}} - USD
        </h3>
        <div class="flex items-end border-gray-600 border-b border-l h-64"
        ref="graph">
           <!-- v-for="(bar,idx) in normGraph()"
          :key="idx" нет кея за которые прицепиться в данных поэтому мы цепляемся заиндекс -->
          <div
          v-for="(bar,idx) in normGraph"
          :key="idx"
          :style="{height:`${bar}%`}"
           class="bg-purple-800 border w-10 "></div>
         
        </div>
        <!-- при нажатии  на крестик  sell=null  -->
        <button
        @click="sell=null"    
        type="button"
          class="absolute top-0 right-0"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            xmlns:svgjs="http://svgjs.com/svgjs"
            version="1.1"
            width="30"
            height="30"
            x="0"
            y="0"
            viewBox="0 0 511.76 511.76"
            style="enable-background:new 0 0 512 512"
            xml:space="preserve"
          >
            <g>
              <path
                d="M436.896,74.869c-99.84-99.819-262.208-99.819-362.048,0c-99.797,99.819-99.797,262.229,0,362.048    c49.92,49.899,115.477,74.837,181.035,74.837s131.093-24.939,181.013-74.837C536.715,337.099,536.715,174.688,436.896,74.869z     M361.461,331.317c8.341,8.341,8.341,21.824,0,30.165c-4.16,4.16-9.621,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    l-75.413-75.435l-75.392,75.413c-4.181,4.16-9.643,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    c-8.341-8.341-8.341-21.845,0-30.165l75.392-75.413l-75.413-75.413c-8.341-8.341-8.341-21.845,0-30.165    c8.32-8.341,21.824-8.341,30.165,0l75.413,75.413l75.413-75.413c8.341-8.341,21.824-8.341,30.165,0    c8.341,8.32,8.341,21.824,0,30.165l-75.413,75.413L361.461,331.317z"
                fill="#718096"
                data-original="#000000"
              ></path>
            </g>
          </svg>
        </button>
      </section>
    </div>
  </div>
</template>

<script>
import AddBlock from './components/AddBlock.vue'
export default {
  components:{
    AddBlock
  },
  props:{
    disabled: {
    type:Boolean,
    required: false,
    default: false
  }},
  data(){
    return{
    blockItems:[],
    filterData: "",
    sell:null ,// состояние, изначально равно 0
    graph:[], //график
    page: 1,
    maxGraphEl:1
    }
  },
  created(){ 
    const blockItemsData=localStorage.getItem("cryptonomicon-list");
    if(blockItemsData){
      this.blockItems = JSON.parse(blockItemsData)
      this.blockItems.forEach(ticker => {
        this.update(ticker.name);// для каждого элемента загруженного из json.parse подписывамся на обновления 
      });

    }
  },
  // тут описываются функции для событий и прочего
watch: {
  blockItems(){ //когда меняются блоки с элементами сохранить изменение в локалсторэдж
   return localStorage.setItem("cryptonomicon-list",JSON.stringify(this.blockItems));// локальное хранилище данных в браузере получаем эти данные по ключу, и значению 
  },
  sell(){
this.graph=[];// когда измен выбранный блок сбрасывание графика
this.$nextTick(()=>this.calcMaxGraphElements());// вызов колбэка сразу после изменения элемента  
},
  separationBlockItems(){
if(this.separationBlockItems.length===0&&this.page>1){// при удалениии эл-тов на 2 стр. будет перебрасывать на первую страницу 
  this.page-=1;
}
  },
  filterData() {//  когда меняется поле фильтр сбросить стр на 1
    this.page = 1;
  }
},
mounted() {
  window.addEventListener('resize', this.calcMaxGraphElements)
},
beforeUnMount(){
  window.removeEventListener('resize', this.calcMaxGraphElements)
},
computed: {
  // manySymbol(){
  //   return this.blockItems.length > 4;
  // },
  normGraph(){
  const maxValue=Math.max(...this.graph);
  const minValue=Math.min(...this.graph);
  if(maxValue===minValue){
  return this.graph.map(()=>50);// все значения в графике одинаковые, одинаковые средние линии 
}
return this.graph.map((price)=> 5+((price-minValue)*95)/(maxValue-minValue))

},
  startIndex(){
   return (this.page -1)*6;
  },
  endIndex(){
    return this.page*6;
  },
  filteredList () {
return this.blockItems.filter(el => el.name.includes(this.filterData));
},
separationBlockItems(){
  return this.filteredList.slice(this.startIndex, this.endIndex);
},
hasNextPage(){
  return this.filteredList.length>this.endIndex;

}
},
methods:{ 
   calcMaxGraphElements(){
    if(!this.maxGraphEl ){
      return;
    }
this.maxGraphEl=this.$refs.graph.clientWidth/38;
   },
  update (newTikerName) {
    setInterval( async () => { 
  const response= await fetch( // работа с асинхронной функцией, await заставит интерпретатор  ждать  пока промис справа от await не выполнится т.е fetch
  `https://min-api.cryptocompare.com/data/price?fsym=${newTikerName}&tsyms=USD`)
  const data=await response.json() 
  this.blockItems.find(el=>el.name===newTikerName).price=data.USD
  if(this.sell?.name===newTikerName){
  this.graph.push(data.USD)
  if (this.graph.length > this.maxGraphEl){
    this.graph.shift();
  }
}
}, 300); //в массиве нашел элемент с именем равным такому же как newTiker.name и добавил ему значение в $ нужной вылюты крипто
this.nameBlock="" // после добавления данных поле становится чистым 
},
add (nameBlock) {
const newTiker={
name:nameBlock,
price:"-" // данные введенные в инпут добавляются в заголовок блока
}; 
this.blockItems=[...this.blockItems,newTiker]//добавление в массив новых эл-в и обновление ссылки на массив
this.filterData = "";// при начале ввода поле фильтра filterData становится чистым 
this.update(newTiker.name);
},

select(nameBlock){
this.sell=nameBlock; //при выборе нового блока график чистится(становится также новым ) 
},
remove(deleteBlokItems){
this.blockItems=this.blockItems.filter(el=> el!== deleteBlokItems);// тут обычный фильтр по массиву для удаления элементов
if(this.sell===deleteBlokItems){
  this.sell=null;
}
}
  }  
}
</script>

<style src="./app.css"></style>
