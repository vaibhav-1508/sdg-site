<template>
  <div @wheel="onwheel">
    <div>
      <div class="carousel">
        <div ref="inner" class="inner" :style="innerStyles">
          <section class="projectCards flex flex-row justify-evenly items-center">
            <div v-for="project in projects" :key="project.name" ref="sections" class="section ">
              <async-card :project="project" />
            </div>
          </section>

        </div>
      </div>

    </div>
    <button @click="prev">prev</button>
    <button @click="next">next</button>
  </div>

</template>

<script>
import { defineAsyncComponent, defineComponent } from '@nuxtjs/composition-api'

export default defineComponent({

    components: {
        asyncCard: defineAsyncComponent(() => import('@/components/Home/projectCard.vue'))
    },
    data() {
        return {
            innerStyles: {},
            step: '',
            projects: null,
            transitioning: false
        }

    },
    async fetch() {
        const fetchedData = await this.$content('index/projectData').fetch()
        this.projects = fetchedData.projects
    },
    mounted() {
        this.setStep()
    },
    methods: {
        setStep() {
            const innerWidth = this.$refs.inner.scrollWidth
            const totalProjects = this.projects.length
            this.step = `${innerWidth / totalProjects}px`
        },

        next() {
            if (this.transitioning) return
            this.transitioning = true
            this.moveLeft()
            this.afterTransition(() => {
                const project = this.projects.shift()
                this.projects.push(project)
                this.resetTranslate()
                this.transitioning = false
            })
        },

        prev() {
            if (this.transitioning) return
            this.transitioning = true
            this.moveRight()
            this.afterTransition(() => {
                const project = this.projects.pop()
                this.projects.unshift(project)
                this.resetTranslate()
                this.transitioning = false
            })
        },

        resetTranslate() {
            this.innerStyles = {
                transition: 'none',
                transform: `translateX(-${this.step})`
            }
        },

        afterTransition(callback) {
            const listener = () => {
                callback()
                this.$refs.inner.removeEventListener('transitionend', listener)
            }
            this.$refs.inner.addEventListener('transitionend', listener)
        },

        moveLeft() {
            this.innerStyles = {
                transform: `translateX(-${this.step})
                    translateX(-${this.step})`
            }
        },
        moveRight() {
            this.innerStyles = {
                transform: `translateX(${this.step})
                    translateX(-${this.step})`
            }
        },
        onwheel() {
            if (event.deltaY > 0)
                this.prev()
            if (event.deltaX > 0)
                this.next()
            else if (event.deltaX < 0)
                this.prev()
        }

    }
})
</script>

<style lang="scss" scoped>
.carousel {
    margin-bottom: 0;
}

.section {
    margin-left: 50px;
    margin-right: 50px;
}

.inner {
    transition: transform 0.2s;
    white-space: nowrap;
}
</style>