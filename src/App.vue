<template>
  <div style="display: flex; justify-content: center">
    <input v-model="firstData">
    <input v-model="secondData">
    <div style="display: flex; height: 32px; width: 32px; justify-content: center;
    align-items: center; background-color: green; color: white; cursor: pointer" @click="setRange(firstData, secondData)">OK</div>
  </div>
  <div style="height: 50px; display: flex"></div>
  <div>Courses List:</div>
  <div v-for="course in courses" :key="course">{{course}}</div>
  <div style="height: 100px; display: flex"></div>
  <div>{{`Filtered List[${range}]:`}}</div>
  <div v-for="course in coursesFilter(range)" :key="course">{{course}}</div>
</template>

<script>

export default {
  name: 'App',
  components: {},
  data(){
    return{
      courses: [
        { name: "Courses in England", prices: [0, 100] },
        { name: "Courses in Germany", prices: [500, null] },
        { name: "Courses in Italy", prices: [100, 200] },
        { name: "Courses in Russia", prices: [null, 400] },
        { name: "Courses in China", prices: [50, 250] },
        { name: "Courses in USA", prices: [200, null] },
        { name: "Courses in Kazakhstan", prices: [56, 324] },
        { name: "Courses in France", prices: [null, null] },
        { name: "Courses in South Korea", prices: [200, 450] },
        { name: "Courses in Switzerland", prices: [100, 400] },
        { name: "Courses in Poland", prices: [500, 600] },
        { name: "Courses in UAE", prices: [null, 40] },
        { name: "Courses in Denmark", prices: [40, 400] },
        { name: "Courses in Ukraine", prices: [null, 50] },
        { name: "Courses in Japan", prices: [50, 700] },
        { name: "Courses in Philippines", prices: [146, null] },
      ],
      range: [null,null],
      firstData: '',
      secondData: '',
    }
  },
  methods: {
    coursesFilter(range){
      let filteredCoursesList = []

      //проверим значения выбранного диапазона на null и преобразуем их в 0 если это так
      let rangeFirst = !range[0] ? 0 : range[0]
      let rangeSecond = !range[1] ? 0 : range[1]

      filteredCoursesList = this.courses.filter(c => {
        //то же сделаем и со значениями диапазонов цен
        let first = !c.prices[0] ? 0 : c.prices[0]
        let second = !c.prices[1] ? 0 : c.prices[1]

        if((rangeFirst === 0 || rangeFirst < 0) && rangeSecond > 0){
          if((first >= 0 && first < rangeSecond) && second <= rangeSecond)
            return first >= 0 && second <= rangeSecond
        }
        else if(rangeFirst > 0 && (rangeSecond === 0 || rangeSecond < 0)){
          if(first >= rangeFirst)
            return first >= rangeFirst
        }
        else if((rangeFirst === 0 || rangeFirst < 0) && (rangeSecond === 0 || rangeSecond < 0))
          return true
        else if(rangeFirst > 0 && rangeSecond > 0){
          if(first >= rangeFirst && second <= rangeSecond){
            if(first === 0 && (second >= 0 && second >= rangeFirst))
              return first === 0 && (second >= 0 && second <= rangeSecond)
            else if((first >= 0 && first <= rangeSecond) && second === 0)
              return (first >= 0 && first <= rangeSecond) && second === 0
            else if(first > 0 && second > 0)
              return (first >= rangeFirst && second <= rangeSecond)
            return false
          }
          else if(first === 0 && second === 0)
            return true
        }
        return false
      })

      //сразу же применим сортировку к отфильтрованному списку
      return this.coursesSort(filteredCoursesList)
    },
    coursesSort(courses){
      return courses.sort(function(a, b){
        return a.name.localeCompare(b.name)
      })
    },
    setRange(first, second){
      //проверка того что первое значение больше второго
      if(first > second){

        //если второе поле пусто, то возможно юзер хотел указать только начальное значение диапазона
        if(first.length > 0 && second.length === 0){
          this.range = [parseInt(first), 0]
        }
        else{//иначе если всё же есть 2 значения
          //поменяем их местами(как вариант) если первое больше
          [this.firstData, this.secondData] = [this.secondData, this.firstData]

          //ну и раз уж мы изменили исходные данные, то прямо их и запишем в диапазон
          this.range = [parseInt(this.firstData), parseInt(this.secondData)]
        }
      }
      else if(first < second){

        //если первое поле пусто, то возможно юзер хотел указать только конечное значение диапазона
        if(first.length === 0 && second.length > 0){
          this.range = [0, parseInt(second)]
        }
        else{
          this.range = [parseInt(first), parseInt(second)]
        }
      }
      else{
        if(first.length === 0)//пусто ли поле 1
          first = 0
        if(second.length === 0)//пусто ли поле 2
          second = 0
        this.range = [parseInt(first), parseInt(second)]
      }
    }
  },
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
