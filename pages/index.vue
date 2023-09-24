<template>
  <div class="space-y-10 p-8">
    <!-- Select CD -->
    <div class="flex space-x-4">
      <button 
      v-for="course in courses" 
      @click="selectCourse(course)" 
      class="flex border rounded border-black">
        {{course}}
      </button>
    </div>
    <!-- Main -->
    <div class="flex w-full h-[500px]">
      <!-- Select concept -->
      <div class="w-1/2 flex flex-col space-y-4 overflow-y-scroll">
        <div 
        v-for="concept in courseSelected" 
        @click="selectConcept(concept.id)" 
        class="rounded bg-gray-500 justify-center text-center"
        :class="concept.id === conceptSelected.id ? `bg-green-500` : ``">
          {{concept.data.english}}
        </div>
      </div>
      <!-- Display concept -->
      <div class="w-1/2 flex justify-center items-center space-x-5">
        <div>{{ conceptSelected.data.translation }}</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">

interface APIBody {
    /* properties defined here */
    data: Object
  }

interface Concept {
    /* properties defined here */
    id: Number,
    type: String,
    data: Object
  }

type Courses = 'CD1' | 'CD2' | 'CD3' | 'CD4' | 'CD5';


const courses = ref<String[]>([])
const courseSelected = ref<Concept[]>([
    {
      'id': 0,
      'type': 'null',
      'data': {'english': 'test', translation: 'test'}
    }
  ]
)

const conceptSelectedId = ref<Number>()
const conceptSelected = ref<Concept>(
  { "id": 0,
    "type": "",
    "data": { 
      "english": "",
      "translation": "" 
    } 
  })

// static file in public dir
const { data } = await useFetch<APIBody >('http://localhost:3000/data.json')
courses.value = Object.keys( data.value.data )

function selectCourse(course:Courses){
  courseSelected.value = data?.value?.data[course]['Part 2']
  conceptSelected.value = 
    { "id": 0,
      "type": "",
      "data": { 
        "english": "",
        "translation": "" 
      } 
    }
}

function selectConcept(id:Number){
  conceptSelected.value = courseSelected.value.filter(obj => { 
    return obj.id === id
  })[0]
}
</script>