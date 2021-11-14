<template>
    <div>
        <div 
            v-for="h in headers.default"
            :key="h.id"
            class="menu-item"
            :class="`${activeState < 0 ? '' : activeState === h.id ? 'active' : 'hidden'}`"
            >
            <h2 
                @click="forward(h.id)">{{ h.name }}</h2>
            <span 
                :style="`opacity: ${activeState === h.id ? 1 : 0};`"
                @click="back(h.id)">←</span>
        </div>
        <div
            class="menu-item"
            :class="`${activeState < 0 ? '' : 'hidden'}`">
            <h2>
                <span 
                    @click="$emit('changeLang', 'en')"
                    :style="`opacity: ${lang === 'en' ? .2 : 1};`">English</span>&nbsp;
                /&nbsp;
                <span 
                    @click="$emit('changeLang', 'ru')"
                    :style="`opacity: ${lang === 'ru' ? .2 : 1};`">Русский</span>
            </h2>
        </div>
    </div>
</template>

<script>
import * as enHeaders from "@/assets/lang/headers/en_US.json"
import * as ruHeaders from "@/assets/lang/headers/ru_RU.json"
export default {
    props: {
        lang: {
            type: String,
            required: true
        }
    },
    data: () => ({
        activeState: -1,
        headers: enHeaders,
        angle: 0
    }),
    methods: {
        back(id) {
            this.activeState = -1
            this.$emit("goBack", id)
        },
        forward(id) {
            this.activeState = id
            /*
            * Надо что-то сделать с анимациями меню:
            * 1) Убрать все элементы меню, кроме активного; +
            * 2) Отцентрировать активный элемент меню.
            */
            this.$emit("goForward", id)
        },
        scrollHandler() {
            this.angle = window.scrollY
            this.$emit('rotateElements')
        }
    },
    watch: {
        lang(val) {
            this.headers = (val === 'en') ? enHeaders : ruHeaders
        }
    },
    computed: {
        matrArgs: function() { return [
            [Math.round(Math.cos(this.angle * Math.PI / 180) * 10000) / 10000,
             Math.round(Math.sin(this.angle * Math.PI / 180) * 10000) / 10000,
             0, 0],
            [-Math.round(Math.sin(this.angle * Math.PI / 180) * 10000) / 10000,
             Math.round(Math.cos(this.angle * Math.PI / 180) * 10000) / 10000,
             0, 0],
            [0, 0, 1, 0],
            [0, 0, 0, 1]
        ]}
    },
    mounted() {
        window.addEventListener('scroll', this.scrollHandler)
    },
    unmounted() {
        window.removeEventListener('scroll', this.scrollHandler)
    }
}
</script>

<style scoped>
.active {
    opacity: .2;
}
.active:hover {
    opacity: .6;
}
.hidden {
    opacity: 0;
}
</style>