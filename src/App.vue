<template>
    <div id="app">
        <div class="widget" ref="screenBox">
            <div class="menuToggle" @click="expanded = false">
                <a class="c-hamburger c-hamburger--htra" :class="{'is-active': expanded}">
                    <span></span>
                </a>
            </div>
            <div class="messageElement" :style="elWrapStyle.mes" @click="expanded = false" :class="{'active': expanded && !dragging}">
                <span :class="{'slide-span': expanded && !dragging}">Messages</span>
            </div>
            <div class="elementsWrap"
                 :style="elWrapStyle.wrap"
                 @click="expanded = true"
                 :class="{'slide-in-bottom': expanded && !dragging, 'slide-in-top': !expanded && !dragging, 'transited': !dragging}"
                 draggable="true"
                 @drag="dragElement"
                 @dragend="dragEnd"
                 @touchmove="touchElement"
                 @touchend="touchEnd"
                 ref="elementsBox">

                <div class="topElement" :class="{'slide-in-bottom-2': expanded}">
                    <div class="elem"
                         v-for="(elem, index) in slidingElements"
                         :style="'elWrapStyle.elem'+index"
                         :class="{'active': elem.elem == 'active',
                         'next-slide': elem.elem == 'next-slide',
                         'next-slide-hidden': elem.elem == 'next-slide-hidden',
                         'transited': !dragging,
                         'expanded-slider': expanded}">

                    </div>
                </div>
                <div class="bottomElement" :class="{'fade-in': expanded}">

                </div>
            </div>
        </div>
    </div>
</template>

<script>

    export default {
        name: 'app',
        data () {
            return {
                slidingElements: [
                    {elem: 'active'},
                    {elem: 'next-slide'},
                    {elem: 'next-slide-hidden'}
                ],
                expanded: false,
                bodyHeight: this.getWrapHeight,
                dragging: false,
                elWrapStyle: {mes: {}, wrap: {}}
            }
        },
        methods: {
            dragElement(e){
                this.dragging = true;
                if (e.pageY != 0) {
                    var yPos = e.pageY;
                    var botPos = yPos / this.getWrapHeight * 100;
                    this.elWrapStyle.mes = {'opacity': (100 - botPos) * 0.01};
                    if (botPos < 90 && botPos > 7) {
                        this.elWrapStyle.wrap = {
                            'top': '' + botPos + '%',
                            'width': '' + (95 + (((100 - botPos) * 0.1) / 2)) + '%',
                            'margin': '' + (2.5 - ((100 - botPos) * 0.1) / 4 + '%')
                        };
                        this.$refs.elementsBox.childNodes[2].style.opacity = (((100 - botPos) * 0.01) - 0.2)
                        if ((100 - botPos) > 33 && (100 - botPos) < 90) {
                            this.$refs.screenBox.childNodes[2].children[0].style.top = (botPos - 6) / 2 + '%'
                            if ((100 - botPos) > 75) {
                                this.$refs.screenBox.childNodes[2].children[0].style.left = (((100 - botPos)) * 0.1) * 2 + '%'
                                this.$refs.screenBox.childNodes[2].children[0].style.fontSize = (botPos * 0.1) + 'em'
                            }
                        }
                        if ((100 - botPos) < 77 && (100 - botPos) > 15) {
                            this.$refs.elementsBox.childNodes[0].childNodes[1].style.left = (100 - botPos) + '%'
                        }
                        if ((100 - botPos) < 144 && (100 - botPos) > 30) {
                            this.$refs.elementsBox.childNodes[0].childNodes[2].style.left = ((100 - botPos) * 2) + '%'
                        }
                    }
                }
            },
            dragEnd(e){
                var yPos = e.pageY
                var botPos = yPos / this.getWrapHeight * 100
                if (botPos > 55) {
                    this.expanded = false
                    this.elWrapStyle = {mes: {}, wrap: {}}
                } else {
                    this.expanded = true
                    this.elWrapStyle = {mes: {}, wrap: {}}
                }
                this.$refs.elementsBox.childNodes[0].childNodes[2].style.left = '';
                this.$refs.elementsBox.childNodes[0].childNodes[1].style.left = '';
                this.$refs.screenBox.childNodes[2].children[0].style.left = '';
                this.$refs.screenBox.childNodes[2].children[0].style.fontSize = '';
                this.$refs.screenBox.childNodes[2].children[0].style.top = '';
                this.dragging = false

            },
            touchElement(e){
                this.dragging = true;
                if (e.changedTouches[0].pageY != 0) {
                    var yPos = e.changedTouches[0].pageY;
                    var botPos = yPos / this.getWrapHeight * 100;
                    this.elWrapStyle.mes = {'opacity': (100 - botPos) * 0.01};
                    if (botPos < 90 && botPos > 7) {
                        this.elWrapStyle.wrap = {
                            'top': '' + botPos + '%',
                            'width': '' + (95 + (((100 - botPos) * 0.1) / 2)) + '%',
                            'margin': '' + (2.5 - ((100 - botPos) * 0.1) / 4 + '%')
                        };
                        this.$refs.elementsBox.childNodes[2].style.opacity = (((100 - botPos) * 0.01) - 0.2)
                        if ((100 - botPos) > 33 && (100 - botPos) < 90) {
                            this.$refs.screenBox.childNodes[2].children[0].style.top = (botPos - 6) / 2 + '%'
                            if ((100 - botPos) > 75) {
                                this.$refs.screenBox.childNodes[2].children[0].style.left = (((100 - botPos)) * 0.1) * 2 + '%'
                                this.$refs.screenBox.childNodes[2].children[0].style.fontSize = (botPos * 0.1) + 'em'
                            }
                        }
                        if ((100 - botPos) < 77 && (100 - botPos) > 15) {
                            this.$refs.elementsBox.childNodes[0].childNodes[1].style.left = (100 - botPos) + '%'
                        }
                        if ((100 - botPos) < 144 && (100 - botPos) > 30) {
                            this.$refs.elementsBox.childNodes[0].childNodes[2].style.left = ((100 - botPos) * 2) + '%'
                        }
                    }
                }
            },
            touchEnd(e){
                var yPos = e.changedTouches[0].pageY
                var botPos = yPos / this.getWrapHeight * 100
                if (botPos > 55) {
                    this.expanded = false
                    this.elWrapStyle = {mes: {}, wrap: {}}
                } else {
                    this.expanded = true
                    this.elWrapStyle = {mes: {}, wrap: {}}
                }
                this.$refs.elementsBox.childNodes[0].childNodes[2].style.left = '';
                this.$refs.elementsBox.childNodes[0].childNodes[1].style.left = '';
                this.$refs.screenBox.childNodes[2].children[0].style.left = '';
                this.$refs.screenBox.childNodes[2].children[0].style.fontSize = '';
                this.$refs.screenBox.childNodes[2].children[0].style.top = '';
                this.dragging = false
            }

        },
        beforeMount: {},
        computed: {
            getWrapHeight(){
                this.bodyHeight = this.$refs.screenBox.clientHeight
                return this.bodyHeight
            }
        }
    }
</script>

<style>
@import "style.css";
</style>
