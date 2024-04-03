<template>
  <div class="flex flex-col h-screen  md:justify-center items-center p-6 ">

    <div class="flex flex-col w-full  justify-center items-center" v-if="currentIndex < question.length">
      <h1 class=" text-2xl  font-bold text-center">{{ question[currentIndex] }}</h1>
      <input type="text" class="border-2 border-gray-300 p-2 rounded-lg md:w-1/2 w-full mt-4" placeholder="Answer"
        v-model="answer">

      <!-- text field next quest save my answer  -->
      <div class="flex flex-row">
        <!-- <button class="bg-blue-500 text-white p-2 rounded-lg mt-4 px-4">Save</button> -->
        <button class="bg-blue-500 text-white p-2 rounded-lg mt-4 ml-4 px-4" @click="save">Save & Next</button>
      </div>
    </div>

    <div class="flex flex-col justify-center" v-if="currentIndex === question.length">
      <!--buttlet point question and answers from  loop-->
      <h1 class=" text-2xl  font-bold mt-4">Your answers: {{ formattedDate  }}</h1>
      
        <div v-for="(value, key) in answers" :key="key" class="mt-4 flex flex-col">
          <span class="font-bold text-gray-500">{{ key }}</span> 
          <span>Answer: {{ value }}</span>

        </div>

      <!-- Thank you -->
      <h1 class=" text-2xl  font-bold mt-4">Thank you for your time</h1>
      <button v-if="!isPrint" class="bg-blue-500 text-white p-2 rounded-lg mt-4 px-4" @click="print">Download my pdf</button>
      <button v-if="!isPrint" class=" bg-gray-400 text-white p-2 rounded-lg mt-4 px-4" @click="reset">Redo My Question</button>
      <div class=" h-6"></div>
    </div>

  </div>
</template>

<script setup lang="ts">

useHead({
  title: "10 Journalling Prompts That Changed My Life",
});

/// save the answer in to list for each question
/// download pdf

const date = new Date(Date.now());
const formattedDate = date.toLocaleDateString('en-US', {
  year: 'numeric',
  month: 'long',
  day: 'numeric',
  hour: '2-digit',
  minute: '2-digit',
  second: '2-digit'
});


const currentIndex = ref(0)
const answer = ref('')
const isPrint = ref(false)

// onpress next current idnex 
// onpress save save the answer
const next = () => {
  currentIndex.value++
}

// reset current to 0
const reset = () => {
  

  currentIndex.value = 0
}

// print window print
const print = () => {
  isPrint.value = true
  // delay 2 second
  setTimeout(() => {
    window.print()
    
  }, 200)
  setTimeout(() => {
    isPrint.value = false
  }, 2000)
}

const answers: Ref<{ [key: string]: string }> = ref({});

// save and next take value from text fiedl and save it to answers
const save = () => {


  // return if empty answer
  if (answer.value === "") {
    alert("Please enter answer");
    return
  }

  answers.value[question[currentIndex.value]] = answer.value;
  answer.value = "";
  next();
}



const question: string[] = [
  "1. What would you do if money were no object?  1. How would you use your talents and skills to serve others?",
  "2 . What would you like people to say at your funeral? ",
  "3. If I repeat this week's actions for 10 years, where does it lead, and is that where I want to be? ",
  "4. What activities in the last 2 weeks have energized and drained me? ",
  "5. How is your wheel of life? Work, health and relationship",
  "6. Odyssey plan - cureent path 5 years, alternative path, radical path ",
  "7. What is the goal and what is the bottleneck? ",
  "8. Which goal will have the greatest impact on your life? ",
  "9. Do you work for business or does it work for you? ",
  "10. If I knew I was going to die 2 years from now, how would I spend my time?",
]

</script>