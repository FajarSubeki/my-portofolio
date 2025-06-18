<script setup>
import Layout from "../Layout.vue";
import {projectsData} from "../Utils/ProjectsData.js";
import {Icon} from "@iconify/vue";
import {styleGuide} from "../Utils/ReusableStyles.js";
import { ref } from 'vue';

import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'
import 'swiper/css/navigation'
import 'swiper/css/autoplay'

import { Navigation, Autoplay } from 'swiper/modules'

// Create a reactive state to track expanded descriptions
const expandedDescriptions = ref(
    projectsData.reduce((acc, project) => {
      acc[project.name] = false;
      return acc;
    }, {})
);

// Function to toggle description
const toggleDescription = (projectName) => {
  expandedDescriptions.value[projectName] = !expandedDescriptions.value[projectName];
};

// Modal

const selectedProject = ref(null);
const showModal = ref(false);

const openModal = (project) => {
  selectedProject.value = project;
  showModal.value = true;
};

const closeModal = () => {
  showModal.value = false;
  selectedProject.value = null;
};

</script>

<template>
  <Layout id="projects" class="dark:bg-darkBgColor pt-8 pb-24 transition-all duration-500">
    <div class="w-max mx-auto pt-12 flex flex-col items-end">
      <h4 class="text-[2.2rem] font-[600] text-textColor dark:text-darkTextColor text-center">My Portofolio</h4>

      <!--   line   -->
      <div class="w-[50%] h-[2px] bg-highlightColor"></div>
      <div class="w-[70%] h-[3px] bg-highlightPrimary mt-1"></div>
    </div>
    
    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-6 mt-12">
      <div v-for="project in projectsData"
          :key="project.name"
          @click="openModal(project)"
          class="flex flex-col items-center p-4 bg-white rounded-2xl transition duration-300 h-[299px]"
          style="box-shadow: 0 2px 4px rgba(0,0,0,0.08), 0 -2px 4px rgba(0,0,0,0.08), 2px 0 4px rgba(0,0,0,0.08), -2px 0 4px rgba(0,0,0,0.08);">
          
        <img :src="project.thumbnail" alt="icon" class="w-45 h-45 object-cover rounded-xl mb-3" />
        
        <h3 class="text-lg font-bold text-gray-800">{{ project.name }}</h3>
        <p class="text-sm text-gray-500">{{ project.type }}</p>
        <div class="border mt-1 rounded-md dark:text-darkDisableColor dark:border-darkBorderColor capitalize px-2 py-1 text-[0.8rem] text-disableColor border-borderColor">
           {{project.tags[0]}}
        </div>
      </div>
    </div>

    <div v-if="showModal" class="fixed inset-0 bg-black bg-opacity-50 flex justify-center items-center z-50">
      <div class="bg-white rounded-2xl shadow-lg pl-10 pt-10 pb-10 pr-10 w-[1200px] max-w-full relative flex gap-6">
        <button @click="closeModal" class="absolute top-2 right-5 text-gray-500 text-4xl">×</button>

        <Swiper
          :modules="[Navigation, Autoplay]"
          :navigation="true"
          :autoplay="{ delay: 3000, disableOnInteraction: false, pauseOnMouseEnter: true }"
          :loop="true"
          :slides-per-view="1"
          :space-between="10"
          class="swiper-custom-nav w-[300px] h-[550px] overflow-hidden"
        >
          <SwiperSlide
            v-for="(img, index) in selectedProject.gallery"
            :key="index">
            <img :src="img" class="w-full h-full object-cover rounded-2xl" />
          </SwiperSlide>
        </Swiper>

        <!-- Content on the right -->
        <div class="flex-1 flex flex-col">
            <!-- Row: icon + title -->
            <div class="flex items-center justify-between mb-2">
              <div class="flex items-center gap-3">
                <div class="w-12 h-12 rounded-md overflow-hidden flex-shrink-0">
                  <img :src="selectedProject.thumbnail" alt="icon" class="w-full h-full object-cover" />
                </div>
                <h2 class="text-3xl font-bold text-gray-800">{{ selectedProject?.name }} - {{ selectedProject?.type }} </h2>
              </div>
            </div>

            <div class="flex items-center flex-wrap gap-[8px] mt-3">
              <p class="text-[1rem] font-[600] dark:text-darkDisableColor text-disableColor">Technology:</p>
              <div v-for="tag in selectedProject.tags" :key="tag" class="flex items-center gap-[15px] flex-wrap">
                <div class="border rounded-md dark:text-darkDisableColor dark:border-darkBorderColor capitalize px-2 py-1 text-[0.8rem] text-disableColor border-borderColor">
                  {{tag}}
                </div>
              </div>
            </div>

            <div class="flex items-center justify-between mb-2 mt-3">
              <div class="flex items-center gap-2">
                <div class="w-5 h-5 rounded-md overflow-hidden flex-shrink-0">
                  <img src="https://d1nhio0ox7pgb.cloudfront.net/_img/o_collection_png/green_dark_grey/512x512/plain/book.png" alt="icon" class="w-full h-full object-cover" />
                </div>
                <h2 class="text-2xl font-bold text-gray-800">About</h2>
              </div>
            </div>

            {{selectedProject.description}}

            <div class="flex items-center justify-between mb-2 mt-5">
              <div class="flex items-center gap-2">
                <div class="w-5 h-5 rounded-md overflow-hidden flex-shrink-0">
                  <img src="https://static-00.iconduck.com/assets.00/globe-icon-2048x2048-la3vxx3a.png" alt="icon" class="w-full h-full object-cover" />
                </div>
                <h2 class="text-2xl font-bold text-gray-800">Find In :</h2>
              </div>
            </div>

            <div class="flex items-center gap-2 mt-3 mb-2">
              <a
                :href="selectedProject?.playstore"
                target="_blank"
                rel="noopener noreferrer"
                class="cursor-pointer"
              >
                <img
                  src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/78/Google_Play_Store_badge_EN.svg/1024px-Google_Play_Store_badge_EN.svg.png"
                  alt="Play Store"
                  class="w-[160px] h-[48px] object-contain"
                />
              </a>

              <a
                :href="selectedProject?.appstore"
                target="_blank"
                rel="noopener noreferrer"
                class="cursor-pointer"
              >
                <img
                  src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/3c/Download_on_the_App_Store_Badge.svg/2560px-Download_on_the_App_Store_Badge.svg.png"
                  alt="App Store"
                  class="w-[160px] h-[48px] object-contain"
                />
              </a>
            </div>

          </div>
        </div>

    </div>

    <!-- <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-[20px] mt-12">
      <div v-for="project in projectsData" :key="project.name" class="border dark:border-darkBorderColor border-borderColor rounded-md">

        <div class="h-[150px] lg:h-[210px] overflow-hidden rounded-t-md">
          <img
              :alt="project.name"
              :src="project.thumbnail"
              class="rounded-t-md hover:scale-[1.1] transition-all duration-500 object-cover"
          />
        </div>

        <div class="p-4">
          <h3 class="text-[1.3rem] dark:text-darkTextColor text-textColor font-[600]">{{project.name}}</h3>

          <div class="text-[0.9rem] font-[400] dark:text-darkDisableColor text-disableColor mt-1">

            <template v-if="!expandedDescriptions[project.name]">
              {{project.description.slice(0, 90)}}
              <span v-if="project.description.length > 100" class="text-highlightPrimary/80 underline cursor-pointer ml-1"
                    @click="toggleDescription(project.name)">
                See More
              </span>
            </template>

            <template v-else>
              {{project.description}}
              <span class="text-highlightPrimary/80 underline cursor-pointer ml-1"
                    @click="toggleDescription(project.name)">
                See Less
              </span>
            </template>

          </div>

          <div class="flex items-center flex-wrap gap-[8px] mt-3">
            <p class="text-[1rem] font-[600] dark:text-darkDisableColor text-disableColor">Technology:</p>
            <div v-for="tag in project.tags" :key="tag" class="flex items-center gap-[15px] flex-wrap">
              <div class="border rounded-md dark:text-darkDisableColor dark:border-darkBorderColor capitalize px-2 py-1 text-[0.8rem] text-disableColor border-borderColor">
                {{tag}}
              </div>
            </div>
          </div>

          <div class="flex items-center gap-[10px] mt-5">
            <a :class="styleGuide.buttonFill" target="_blank" :href="project.live">
              <Icon icon="hugeicons:link-square-02" width="20" height="20" />
              Live
            </a>
            <a v-if="project.github" :class="styleGuide.buttonOutline" target="_blank" :href="project.github">
              <Icon icon="hugeicons:github-01" width="20" height="20" />
              Github
            </a>
            <a v-else :class="[styleGuide.buttonOutline, !project.github && 'dark:bg-slate-800 dark:border-slate-500 cursor-not-allowed bg-gray-100 opacity-50']">
              <Icon icon="hugeicons:github-01" width="20" height="20" />
              Github
            </a>
          </div>

        </div>
      </div>
    </div> -->
  </Layout>
</template>