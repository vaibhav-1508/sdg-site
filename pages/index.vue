<template>
  <div>
    <main>
      <HomeHero />
    </main>

    <section class="w-full flex flex-col justify-evenly">

      <div class="project-carousel">
        <div class="pc w-full flex flex-col justify-evenly">
          <Transition name="fade" @leave="onLeave">
            <div v-if="buttonAction" class="main">
              <div class="info relative my-12 w-2/5">
                <h2 class="text-5xl my-4 text-center font-bold tracking-wide">Projects</h2>
              </div>
              <img src="/home/cops-website.webp" alt="img here">
            </div>
          </Transition>
          <button id="nav-toggle" @click="buttonAction = !buttonAction">
            {{ buttonAction }}
            <i class="open fa-light fa-bars-staggered"></i>
            <i class="close fa-light fa-xmark-large"></i>
          </button>
          <div class="slider flex flex-row justify-center items-center overflow-hidden">
            <HomeProjectCarousel />
          </div>   
        </div>
      </div>








      <transition name="fade">
        <!-- <ul
          v-show="isListVisible"
          ref="jsDots"
          name="fade"
          tag="ul"
          class="
            nav-dots
            fixed
            left-0
            p-4
            m-0
            cursor-pointer
            flex flex-col
            justify-center
            items-center
          "
          @click="scrollToSection"
          @keydown.enter="scrollToSection"
        >
          <li
            v-for="project in projects"
            :key="project.name"
            class="nav-dot list-none text-center text-2xl"
          >
            {{ project.name }}
          </li>
        </ul> -->
      </transition>
      <!-- <transition name="fade">
        <section class="projectCards flex flex-col justify-evenly items-center">
          <div
            v-for="project in projects"
            :key="project.name"
            ref="sections"
            class="section md:min-h-screen"
          >
            <async-card :project="project" />
          </div>
        </section>
      </transition> -->

      <!-- <div id="pc">
        <carousel>
          Numbers 1-10 in divs
          <div v-for="i in [...Array(10).keys()]">{{ i + 1 }}</div>
        </carousel>
      </div> -->

    </section>
  </div>
</template>

<script>
import gsap from 'gsap'
import {  defineComponent } from '@nuxtjs/composition-api'

export default defineComponent({

  data() {
    return {
      isListVisible: false,
      projects: null,
      buttonAction: true,
    }
  },
  async fetch() {
    const fetchedData = await this.$content('index/projectData').fetch()
    this.projects = fetchedData.projects
  },
  head: {
    title: 'Landing',
    meta: [
      {
        hid: 'description',
        name: 'description',
        content:
          'We at Software Development Group are a bunch of designers and developers who aim to encourage the development of technology and innovation in the IIT (BHU) campus (and beyond) by learning, sharing knowledge, and solving problems.'
      }
    ]
  },
  // mounted() {
  //   window.addEventListener('scroll', this.setDotStatus)
  //   window.addEventListener('resize', this.displayList)
  // },
  // unmounted() {
  //   window.removeEventListener('scroll', this.setDotStatus)
  // },
  // computed:{
  //   toggleNav() {
  //     return this.button === "true" ? "false" : "true";
  //   },
  // },
  methods: {

    onLeave(el) {
      gsap.to(el, {
        duration: 0.7,
        y: "-50%",
        ease: 'elastic.inOut(2.5, 1)'
      })
      gsap.to(el, {
        duration: 0.2,
        delay: 0.5,
        opacity: 1,
      })
    },

    displayList() {
      if (window.innerWidth < 1000) {
        this.isListVisible = false
      }
    },
    // removeDotStyles() {
    //   // const dots = this.$refs.jsDots
    //   // const isActive = dots.querySelector('.is-active')

    //   // if (isActive != null) {
    //   //   isActive.classList.remove('is-active')
    //   // }
    // },
    // setDotStatus() {
    //   // const scrollPosition = window.scrollY
    //   // const dots = Array.from(this.$refs.jsDots.children)
    //   // if (
    //   //   scrollPosition > window.innerHeight - 250 &&
    //   //   window.innerWidth > 1000 &&
    //   //   scrollPosition < window.innerHeight * this.projects.length
    //   // ) {
    //   //   this.isListVisible = true
    //   // } else {
    //   //   this.isListVisible = false
    //   // }

    //   // this.$refs.sections.forEach((section, index) => {
    //   //   const halfWindow = window.innerHeight / 2
    //   //   const sectionTop = section.offsetTop
    //   //   if (
    //   //     scrollPosition > sectionTop - halfWindow &&
    //   //     scrollPosition < sectionTop + halfWindow
    //   //   ) {
    //   //     this.removeDotStyles()
    //   //     dots[index].classList.add('is-active')
    //   //   }
    //   // })
    // },
    // scrollToSection(e) {
    //   // const dots = Array.from(this.$refs.jsDots.children)
    //   // const windowHeight = window.innerHeight
    //   // dots.forEach((dot, index) => {
    //   //   if (dot === e.target) {
    //   //     window.scrollTo({
    //   //       top: windowHeight * index + windowHeight + 100,
    //   //       behavior: 'smooth'
    //   //     })
    //   //   }
    //   // })
    // }
  }
})
</script>

<style>
#nav-toggle:hover {
  transform: translateX(-50%) scale(1.04);
}

#nav-toggle:active {
  transform: translateX(-50%) scale(0.96);
}

.main {
  background-position: center 0%;
  height: 100%;
  width: 100%;
  margin: 0;
  position: relative;
  z-index: 2;
  transition: transform 500ms cubic-bezier(.13, .53, .38, .97);
  align-self: center;
}



#nav-toggle {
  height: 5rem;
  width: 5rem;
  position: relative;
  z-index: 3;
  left: 50%;
  margin-top: 0;
  bottom: 3rem;
  transform: translateX(-50%);
  background-color: rgb(122, 218, 144);
  border: none;
  border-radius: 5rem;
  outline: none;
  box-shadow: 0rem 0rem 4rem rgba(0 0 0 / 35%);
  cursor: pointer;
  transition: transform, background-color;
  transition-timing-function: ease;
  transition-duration: 400ms;
}




.info {
  left: 30%;
  color: white;
}

.info h2 {
  color: #00bcd4;
  text-shadow: #046af0 2px 2px 10px;
}

.nav-dots {
  height: 200px;
  width: 400px;
  top: 50%;
  transform: translateY(-50%);
}



.nav-dot {
  color: white;
  transition: all 0.3s;
}

.nav-dot:not(:last-of-type) {
  margin-bottom: 20px;
}

.nav-dot:hover {
  letter-spacing: 3px;
  background: transparent;
}

.nav-dot.is-active {
  transform: scale(1.1);
  letter-spacing: 3px;
  background: transparent;
  color: #89ff00;
}

.fade-enter-active,
.fade-leave-active {
  transform: translateY(-50%);
}

.fade-enter,
.fade-leave-to {
  opacity: 1;
}

@media only screen and (max-width: 1000px) {
  .info {
    left: 5%;
    width: 90%;
  }
}


.slider {
  margin-top: -20%;
  margin-bottom: 5%;
  z-index: 1;
}
</style>
