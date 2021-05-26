<template>
  <div>  
    <div>
      <h1>{{ subjects.length }} Subjects</h1>
      <p>Durchschnitt: {{ Math.round(averageGlobal*2)/2 }} | Mangelpunkte: {{ mangelPunkte }} </p>
      <div class="subject" v-for="subject in subjects">
        <h3> {{ subject.name }}: Ø {{ Math.round(averageGrade(subject.grades)*10)/10 }}</h3>
        <div class="grade" v-for="grade in subject.grades"> {{ grade }} </div>
      </div>
    </div>
    <div class="newGradeContainer">
      <input placeholder="Note" v-model="newGrade">
      <input placeholder="Fach" v-model="chosenSubject">
      <button v-on:click="addGrade">Hinzufügen</button>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data: function() {
      return {
        newGrade: 0,
        chosenSubject: "",
        subjects: [
          {
            name: "Mathematik",
            grades: [5,4.7,3.5,4.1] 
          },
          {
            name: "Geschichte",
            grades: [5.5,4.9,5.75] 
          },
          {
            name: "Physik",
            grades: [2,2.7,4.3] 
          },
          {
            name: "Deutsch",
            grades: [4.25,4.1,3.5] 
          },
        ]
      }
    },
    methods: {
      averageGrade: function(grades) {
        var sum = 0;
        for (var grade of grades) {
          sum += grade
        }
        return parseFloat(sum / grades.length)
      },
      addGrade: function() {
        if (!this.chosenSubject)
          return
        for (var subject of this.subjects) {
          if (subject.name.toLowerCase().includes(this.chosenSubject.toLowerCase()) && this.newGrade >= 1 && this.newGrade <= 6) {
            subject.grades.push(parseFloat(this.newGrade))
            return;
          }
        }
      }
    },
    computed: {
      averageGlobal: function() {
        var sum = 0;
        for (var subject of this.subjects) {
          sum += this.averageGrade(subject.grades)
        }
        return sum / this.subjects.length
      },
      mangelPunkte: function() {
        var mp = 0;
        for (var subject of this.subjects) {
          var subMp = -(Math.round(this.averageGrade(subject.grades)*10)/10 - 4);
          if ( subMp > 0) {
            mp += subMp
          }
        }
        return mp.toPrecision(2);
      }
    },
  }
</script>

<style lang="scss">
  #app {
    font-family: sans-serif;
    color: #000000;
  }

  h1 {
    margin: 0;
  }

  p {
    margin: 0;
  }
</style>