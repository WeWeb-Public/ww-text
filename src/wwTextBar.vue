<template>
    <div class="ww-text-bar" :class="{'show': show}" :style="position" @click="focus()">
        <div class="move" @mousedown="startDrag($event)">
            <span class="fas fa-arrows-alt"></span>
        </div>
        <div class="content" :class="{'expended': expended}">
            <div class="line main">
                <!-- BOLD -->
                <div class="item">
                    <!-- @click="action('exec:bold')" -->
                    <span class="fas fa-bold"></span>

                    <div class="subitems">
                        <div class="item font" @click="setFontWeight(700)">
                            <span>Bold (700)</span>
                        </div>
                        <div class="item font" @click="setFontWeight(400)">
                            <span>Normal (400)</span>
                        </div>
                        <div class="item font" @click="setFontWeight(100)">
                            <span>Light (100)</span>
                        </div>
                        <div class="item input">
                            <input type="text" v-model="d_fontWeight" @click="$event.stopPropagation()" @keydown="checkEnterFontWeight($event)" />
                        </div>
                    </div>
                </div>
                <!-- ITALIC -->
                <div class="item" @click="action('exec:italic')">
                    <span class="fas fa-italic"></span>
                </div>
                <!-- UNDERLINE -->
                <div class="item" @click="action('exec:underline')">
                    <span class="fas fa-underline"></span>
                </div>

                <div class="separator"></div>

                <!-- FONT SIZE -->
                <div class="item">
                    <span class="wwi wwi-font-size"></span>

                    <div class="subitems">
                        <div class="item font-size" v-for="fontSize in c_fontSizes" :key="fontSize.name" @click="action('fontSize:' + getFontSizeClass(fontSize))">{{fontSize.name}}</div>
                        <div class="item font-size blue" @click="action('open:DESIGN_FONT_SIZES')">
                            <span class="wwi wwi-add"></span>
                        </div>
                        <!-- <div class="item input">
                            <input type="text" v-model="customSize" @click="$event.stopPropagation()" @keydown="checkEnterSize($event)" />
                        </div>-->
                    </div>
                </div>
                <!-- FONT -->
                <div class="item">
                    <span class="fas fa-font"></span>

                    <div class="subitems">
                        <div class="item font" v-for="font in c_fonts" :key="font.name" @click="action('exec:font-family:' + getFont(font))" :style="'font-family:' + getFont(font)">{{font.name}}</div>
                        <div class="item font" v-if="getDefaultFont()" @click="action('exec:font-family:inherit')" :style="{'font-family':getDefaultFont()}">
                            - Default -
                            <br />
                            {{ getDefaultFont() }}
                        </div>
                        <div class="item font blue" @click="action('open:DESIGN_FONTS')">
                            <span class="wwi wwi-add"></span>
                        </div>
                        <!-- <div class="item font" @click="action('style:font:more')">
                            <i>More...</i>
                        </div>-->
                    </div>
                </div>
                <!-- ALIGN -->
                <div class="item">
                    <span class="fas fa-align-left"></span>

                    <div class="subitems">
                        <div class="item" @click="action('exec:align:justify')">
                            <span class="fas fa-align-justify"></span>
                        </div>
                        <div class="item" @click="action('exec:align:right')">
                            <span class="fas fa-align-right"></span>
                        </div>
                        <div class="item" @click="action('exec:align:center')">
                            <span class="fas fa-align-center"></span>
                        </div>
                        <div class="item" @click="action('exec:align:left')">
                            <span class="fas fa-align-left"></span>
                        </div>
                    </div>
                </div>
                <!-- COLOR -->
                <div class="item">
                    <span class="wwi wwi-color"></span>
                    <div class="subitems">
                        <div class="item colors">
                            <div class="color">
                                Text
                                <wwManagerColorSelect v-model="d_textColor" @change="setTextColor($event)"></wwManagerColorSelect>
                            </div>
                            <div class="color">
                                Background
                                <wwManagerColorSelect v-model="d_backgroundColor" @change="setBackgroundColor($event)"></wwManagerColorSelect>
                            </div>
                        </div>
                    </div>

                    <!-- <div class="subitems">
                        <div class="item colors">
                            <div style="background-color:white" @click="action('exec:color:white')"></div>
                            <div style="background-color:#808080" @click="action('exec:color:#808080')"></div>
                            <div style="background-color:#404040" @click="action('exec:color:#404040')"></div>
                            <div style="background-color:black" @click="action('exec:color:black')"></div>
                        </div>
                        <div class="item colors">
                            <div style="background-color:#FF0000" @click="action('exec:color:#FF0000')"></div>
                            <div style="background-color:#FF6600" @click="action('exec:color:#FF6600')"></div>
                            <div style="background-color:#FFFF00" @click="action('exec:color:#FFFF00')"></div>
                            <div style="background-color:#8CC700" @click="action('exec:color:#8CC700')"></div>
                        </div>
                        <div class="item colors">
                            <div style="background-color:#0FAD00" @click="action('exec:color:#0FAD00')"></div>
                            <div style="background-color:#00A3C7" @click="action('exec:color:#00A3C7')"></div>
                            <div style="background-color:#0010A5" @click="action('exec:color:#0010A5')"></div>
                            <div style="background-color:#C5007C" @click="action('exec:color:#C5007C')"></div>
                        </div>

                        <div class="item font" @click="action('exec:color:more')">
                            <i>More...</i>
                        </div>
                    </div>-->
                </div>

                <div class="separator"></div>

                <!-- COPY FORMAT -->
                <div class="item" @click="copyFormat()" :class="{'green': d_copiedFormat}">
                    <span class="fas fa-paint-roller"></span>
                </div>

                <div class="separator"></div>

                <!-- EXPEND -->
                <div class="item expend-arrow" @mouseenter="expended = true" :class="{'expended': expended}">
                    <span class="wwi wwi-chevron-down"></span>
                </div>

                <!-- MENU -->
                <div class="item orange round" @click="action('openMenu', $event)">
                    <span class="wwi wwi-edit-other"></span>
                </div>
            </div>
            <div class="line not-main">
                <!-- LINK -->
                <div class="item" @click="action('link')">
                    <span class="fas fa-link"></span>
                </div>
                <!-- CLEAR -->
                <div class="item" @click="action('removeFormat')">
                    <span class="fas fa-remove-format"></span>
                </div>
                <!-- STYLE -->
                <div class="item">
                    <span class="wwi wwi-edit-style"></span>

                    <div class="subitems">
                        <div class="item font-style" v-for="fontStyle in c_fontStyles" :key="fontStyle.name" @click="action('fontStyle:' + fontStyle.className)" :style="fontStyle.style" :class="[fontStyle.style.fontSizeClass, fontStyle.style.color == '#ffffff' ? 'white' : '']">{{fontStyle.name}}</div>
                        <div class="item font-style" @click="action('fontStyle:')">
                            - No style -
                            <br />
                        </div>
                        <div class="item font-style blue" @click="action('open:DESIGN_FONT_STYLES')">
                            <span class="wwi wwi-add"></span>
                        </div>
                        <!-- <div class="item font" @click="action('style:font:more')">
                            <i>More...</i>
                        </div>-->
                    </div>
                </div>

                <div class="separator"></div>

                <!-- BULLETS -->
                <div class="item">
                    <span class="fas fa-list-ul"></span>

                    <div class="subitems">
                        <div class="item" @click="action('indent:right')">
                            <span class="fas fa-outdent"></span>
                        </div>
                        <div class="item" @click="action('indent:left')">
                            <span class="fas fa-indent"></span>
                        </div>
                        <div class="item" @click="action('list:ordered')">
                            <span class="fas fa-list-ol"></span>
                        </div>
                        <div class="item" @click="action('list:bullet')">
                            <span class="fas fa-list-ul"></span>
                        </div>
                    </div>
                </div>
                <!-- LINE SPACE -->
                <div class="item">
                    <span class="fas fa-text-height"></span>

                    <div class="subitems">
                        <div class="item font" @click="setLineHeight(60)">
                            <span>60px</span>
                        </div>
                        <div class="item font" @click="setLineHeight(50)">
                            <span>50px</span>
                        </div>
                        <div class="item font" @click="setLineHeight(40)">
                            <span>40px</span>
                        </div>
                        <div class="item font" @click="setLineHeight(30)">
                            <span>30px</span>
                        </div>
                        <div class="item font" @click="setLineHeight(20)">
                            <span>20px</span>
                        </div>
                        <div class="item input">
                            <input type="text" v-model="d_lineHeight" @click="$event.stopPropagation()" @keydown="checkEnterLineHeight($event)" />
                        </div>
                        <div class="item font" @click="action('exec:lineHeight')">- Default -</div>
                    </div>
                </div>
                <!-- LETTER SPACE -->
                <div class="item">
                    <span class="fas fa-text-width"></span>

                    <div class="subitems">
                        <div class="item font" @click="setLetterSpacing(5)">
                            <span>5px</span>
                        </div>
                        <div class="item font" @click="setLetterSpacing(4)">
                            <span>4px</span>
                        </div>
                        <div class="item font" @click="setLetterSpacing(3)">
                            <span>3px</span>
                        </div>
                        <div class="item font" @click="setLetterSpacing(2)">
                            <span>2px</span>
                        </div>
                        <div class="item font" @click="setLetterSpacing(1)">
                            <span>1px</span>
                        </div>
                        <div class="item input">
                            <input type="text" v-model="d_letterSpacing" @click="$event.stopPropagation()" @keydown="checkEnterLetterSpacing($event)" />
                        </div>
                        <div class="item font" @click="action('exec:letterSpacing')">- Default -</div>
                    </div>
                </div>

                <div class="separator"></div>

                <!-- HEADER -->
                <div class="item">
                    <span :class="getTagClasses()"></span>

                    <div class="subitems">
                        <div class="item" @click="action('prop:tag:h1')">
                            <span class="wwi wwi-tag-h1"></span>
                        </div>
                        <div class="item" @click="action('prop:tag:h2')">
                            <span class="wwi wwi-tag-h2"></span>
                        </div>
                        <div class="item" @click="action('prop:tag:h3')">
                            <span class="wwi wwi-tag-h3"></span>
                        </div>
                        <div class="item" @click="action('prop:tag:h4')">
                            <span class="wwi wwi-tag-h4"></span>
                        </div>
                        <div class="item" @click="action('prop:tag:div')">
                            <span class="fa fa-paragraph"></span>
                        </div>
                    </div>
                </div>

                <div class="separator"></div>

                <!-- LINE -->
                <div class="item">
                    <span class="wwi wwi-separator"></span>

                    <div class="subitems">
                        <div class="item xsmall" @click="insertLine('25%')">
                            <span class="wwi wwi-separator"></span>
                        </div>
                        <div class="item medium" @click="insertLine('50%')">
                            <span class="wwi wwi-separator"></span>
                        </div>
                        <div class="item xbig" @click="insertLine('100%')">
                            <span class="wwi wwi-separator"></span>
                        </div>
                        <div class="item input">
                            <input type="text" v-model="d_lineThickness" @click="$event.stopPropagation()" />
                        </div>
                        <div class="item">
                            <wwManagerColorSelect v-model="d_lineColor" @change="setLineColor($event)"></wwManagerColorSelect>
                        </div>
                    </div>
                </div>
                <!-- WWOBJECT -->
                <div class="item blue" @click="action('insertWwObject')">
                    <span class="wwi wwi-add"></span>
                </div>
            </div>
        </div>
    </div>
</template>


<script>
import Vue from 'vue'
import { setTimeout } from 'timers';


export default {
    name: 'wwTextBar',
    props: {
        options: Object
    },
    data() {
        return {
            show: false,
            expended: false,
            minDragDist: 0,
            dragging: false,
            dragStartPoint: { x: 0, y: 0 },
            dragStartPosition: { x: 0, y: 0 },
            position: {
                top: 0,
                left: 0
            },
            moved: false,
            customSize: 1.5,
            defaultFont: '',
            stopRequestAnimation: false,
            d_copiedFormat: false,
            d_letterSpacing: 0,
            d_lineHeight: 0,
            d_textColor: null,
            d_backgroundColor: null,
            d_lineColor: '#000000',
            d_lineThickness: 1,
            d_fontWeight: 400,
        };
    },
    computed: {
        c_isInLayout() {
            return wwLib.wwUtils.getParentLayout(this.options.context.$el.parentElement);
        },
        c_fonts() {
            let fonts = wwLib.wwWebsiteData.getInfo().fonts || {};
            return fonts.list || [];
        },
        c_fontSizes() {
            let fontSizes = wwLib.wwWebsiteData.getInfo().fontSizes || {};
            return fontSizes.list || [];
        },
        c_fontStyles() {
            let fontStyles = wwLib.wwWebsiteData.getInfo().fontStyles || {};
            return fontStyles.list || [];
        }
    },
    watch: {
    },
    methods: {

        insertLine(width) {
            this.options.context.insertLine({ width: width, color: this.d_lineColor, height: this.d_lineThickness });
        },

        setLineColor(color) {
            this.d_lineColor = color;
        },

        getSpacings() {
            try {
                let format = this.options.context.quill.getFormat();
                if (format.letterSpacing) {
                    this.d_letterSpacing = parseFloat(format.letterSpacing.replace('px', ''));
                }
                else {
                    this.d_letterSpacing = 0;
                }
                if (format.lineHeight) {
                    this.d_lineHeight = parseFloat(format.lineHeight.replace('px', ''));
                }
                else {
                    this.d_lineHeight = 0;
                }
            }
            catch (error) {

            }
        },

        getColors() {
            try {
                let format = this.options.context.quill.getFormat();
                if (format.color) {
                    this.d_textColor = Array.isArray(format.color) ? format.color[0] : format.color;
                }
                else {
                    this.d_textColor = '';
                }
                if (format.background) {
                    this.d_backgroundColor = Array.isArray(format.background) ? format.background[0] : format.background;
                }
                else {
                    this.d_backgroundColor = '';
                }
            }
            catch (error) {
                console.log(error);
            }
        },

        getCopiedFormat() {
            let copiedFormat = null;

            try {
                copiedFormat = localStorage.getItem('ww-text-copied-format');
                copiedFormat = JSON.parse(copiedFormat);
            } catch (error) {

            }

            this.d_copiedFormat = copiedFormat ? true : false;
        },

        copyFormat() {
            this.action('format');

            setTimeout(this.getCopiedFormat, 100);
        },

        focus() {
            // this.options.context.$el.focus();
            // this.options.context.reselect();
        },

        action(action, event) {
            this.options.context.wwTextBarAction(action, event);
        },

        checkEnterSize(event) {
            if (event.keyCode == 13) {
                this.options.context.wwTextBarAction('style:size:' + this.customSize);
            }
        },

        setLetterSpacing(value) {
            this.d_letterSpacing = value;
            this.options.context.wwTextBarAction('exec:letterSpacing' + (this.d_letterSpacing ? ':' + this.d_letterSpacing + 'px' : ''));
        },

        checkEnterLetterSpacing(event) {
            if (event.keyCode == 13) {
                this.setLetterSpacing(this.d_letterSpacing);
            }
        },

        setLineHeight(value) {
            this.d_lineHeight = value;
            this.options.context.wwTextBarAction('exec:lineHeight' + (this.d_lineHeight ? ':' + this.d_lineHeight + 'px' : ''));
        },

        checkEnterLineHeight(event) {
            if (event.keyCode == 13) {
                this.setLineHeight(this.d_lineHeight);
            }
        },

        setFontWeight(value) {
            this.d_fontWeight = value;
            this.options.context.wwTextBarAction('fontWeight:' + value);
        },

        checkEnterFontWeight(event) {
            if (event.keyCode == 13) {
                this.setFontWeight(this.d_fontWeight);
            }
        },

        setTextColor(value) {
            this.d_textColor = value;
            this.options.context.wwTextBarAction('color:' + this.d_textColor);
        },

        setBackgroundColor(value) {
            this.d_backgroundColor = value;
            this.options.context.wwTextBarAction('exec:background:' + this.d_backgroundColor);
        },

        getTagClasses() {
            switch (this.options.context.wwObject.content.data.tag) {
                case 'h1':
                    return ['wwi', 'wwi-tag-h1'];
                    break;
                case 'h2':
                    return ['wwi', 'wwi-tag-h2'];
                    break;
                case 'h3':
                    return ['wwi', 'wwi-tag-h3'];
                    break;
                case 'h4':
                    return ['wwi', 'wwi-tag-h4'];
                    break;
                case 'div':
                    return ['fa', 'fa-paragraph'];
                    break;
            }
        },

        getFont(font) {
            return font.name + ', ' + (font.genericName || 'sans-serif');
        },

        // getFontStyle(fontStyle) {
        //     return fontStyle.name.toLowerCase().replace(/[^a-z]*/g, '');
        // },

        getFontSizeClass(fontSize) {
            return fontSize.name.toLowerCase().replace(/\s/g, '');
        },

        getDefaultFont() {
            const defaultFont = (wwLib.wwWebsiteData.getInfo().fonts || {}).default || {}
            return defaultFont.name;
        },

        /*=============================================m_ÔÔ_m=============================================\
          DRAG TEXT BAR
        \================================================================================================*/

        getOrignialPosition() {
            let width = this.$el.getBoundingClientRect().width;
            let height = this.$el.getBoundingClientRect().height;

            const w = wwLib.getManagerWindow();
            const d = wwLib.getManagerDocument();

            //const scrollX = (w.pageXOffset || d.documentElement.scrollLeft) - (d.documentElement.clientLeft || 0);
            //const scrollY = (w.pageYOffset || d.documentElement.scrollTop) - (d.documentElement.clientTop || 0);

            const wwTextRect = wwLib.wwFrontElements.getRect(this.options.context.$el);
            if (!wwTextRect) {
                return;
            }

            let top = wwTextRect.y - height - 5; //+ scrollY;
            let left = wwTextRect.x// + scrollX;

            if (top < 0) {
                top = wwTextRect.y + wwTextRect.height + 5;
            }

            const innerWidth = (w.innerWidth || d.documentElement.clientWidth || d.getElementsByTagName('body')[0].clientWidth) - 40

            if (left + width > innerWidth) {
                left -= ((left + width) - innerWidth);
            }

            return {
                top: top,
                left: left
            }
        },

        getCurrentPosition() {

            if (this.position.left && this.position.top) {
                return {
                    top: parseInt(this.position.top),
                    left: parseInt(this.position.left)
                }
            }

            return {
                top: 0,
                left: 0
            }
        },

        preventClick(event) {
            event.preventDefault();
            event.stopPropagation();

            return false;
        },
        getEventPosition(event) {
            var position = { x: 0, y: 0 };

            position.x = event.clientX;
            position.y = event.clientY;


            return position;
        },
        isDragging(event) {
            return true;

            if (this.dragging) {
                return true;
            }

            let currentDragPoint = this.getEventPosition(event);

            let dist = Math.pow(currentDragPoint.x - this.dragStartPoint.x, 2) + Math.pow(currentDragPoint.y - this.dragStartPoint.y, 2);

            if (dist > Math.pow(this.minDragDist, 2)) {
                this.dragging = true;
                wwLib.getManagerWindow().addEventListener('click', this.preventClick, true);
                wwLib.getFrontWindow().addEventListener('click', this.preventClick, true);
                return true;
            }

            return false;
        },
        updatePosition() {
            if (this.stopRequestAnimation) {
                return;
            }

            if (this.moved) {
                return;
            }

            const iframePos = wwLib.getManagerDocument().querySelector('.iframe').getBoundingClientRect();

            this.position = {
                top: this.getOrignialPosition().top + iframePos.top + 'px',
                left: this.getOrignialPosition().left + iframePos.left + 'px'
            }

            requestAnimationFrame(this.updatePosition);
        },

        startDrag(event) {
            this.dragStartPoint = this.getEventPosition(event);
            this.dragStartPosition = this.getCurrentPosition();

            wwLib.getManagerWindow().addEventListener('mousemove', this.drag);
            wwLib.getManagerWindow().addEventListener('mouseup', this.stopDrag);

            wwLib.getFrontWindow().addEventListener('mousemove', this.drag);
            wwLib.getFrontWindow().addEventListener('mouseup', this.stopDrag);
        },
        drag(event) {
            if (this.isDragging(event)) {
                this.moved = true;
                document.body.classList.add('no-select');

                let currentDragPoint = this.getEventPosition(event);

                this.position.left = (currentDragPoint.x - this.dragStartPoint.x + this.dragStartPosition.left) + 'px';
                this.position.top = (currentDragPoint.y - this.dragStartPoint.y + this.dragStartPosition.top) + 'px';
            }
        },
        stopDrag(event) {
            this.dragging = false;

            document.body.classList.remove('no-select');
            wwLib.getManagerWindow().removeEventListener('mousemove', this.drag);
            wwLib.getManagerWindow().removeEventListener('mouseup', this.stopDrag);
            wwLib.getManagerWindow().removeEventListener('click', this.preventClick, true);

            wwLib.getFrontWindow().removeEventListener('mousemove', this.drag);
            wwLib.getFrontWindow().removeEventListener('mouseup', this.stopDrag);
            wwLib.getFrontWindow().removeEventListener('click', this.preventClick, true);
        }

    },
    created() {
    },
    mounted() {

        this.updatePosition();

        this.$nextTick(function () {
            this.show = true;
        })

        this.getCopiedFormat();
        this.getSpacings();
        this.getColors();

        this.options.context.quill.on('selection-change', () => {
            setTimeout(() => {
                if (this.options.context.focus) {
                    this.getSpacings();
                    this.getColors();
                }
            }, 200)

        });

    },
    beforeDestroy() {
        this.stopRequestAnimation = true;

        wwLib.getManagerWindow().removeEventListener('mousemove', this.drag);
        wwLib.getManagerWindow().removeEventListener('mouseup', this.stopDrag);
        wwLib.getManagerWindow().removeEventListener('click', this.preventClick, true);

        wwLib.getFrontWindow().removeEventListener('mousemove', this.drag);
        wwLib.getFrontWindow().removeEventListener('mouseup', this.stopDrag);
        wwLib.getFrontWindow().removeEventListener('click', this.preventClick, true);
    }
};
</script>

<style scoped lang="scss">
$ww-red: #e02a4d;
$ww-green: #49b9b3;
$ww-orange: #ef811a;
$ww-blue: #2e85c2;

.ww-text-bar {
    position: absolute;
    z-index: 50;
    top: 0;
    left: 0;
    opacity: 1;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    //box-shadow: 0 1px 4px 0 rgba(0, 0, 0, 0.3);
    -webkit-filter: drop-shadow(0 2px 5px rgba(99, 99, 99, 0.5));
    filter: drop-shadow(0 2px 5px rgba(99, 99, 99, 0.5));
    -webkit-transition: opacity 0.2s ease,
        -webkit-transform 0.2s cubic-bezier(0.35, 0.07, 0.35, 1.65);
    transition: opacity 0.2s ease,
        -webkit-transform 0.2s cubic-bezier(0.35, 0.07, 0.35, 1.65);
    transition: transform 0.2s cubic-bezier(0.35, 0.07, 0.35, 1.65),
        opacity 0.2s ease;
    transition: transform 0.2s cubic-bezier(0.35, 0.07, 0.35, 1.65),
        opacity 0.2s ease,
        -webkit-transform 0.2s cubic-bezier(0.35, 0.07, 0.35, 1.65);

    /*
    .handle-container {
        cursor: move;
        cursor: grab;

        .handle {
            display: flex;
            justify-content: center;
            width: 30px;
            color: black;
            position: relative;
            flex-direction: column;
            margin: 30px 0;
            height: 28px;

            div {
                flex-basis: 33.33%;
                position: relative;

                &::after,
                &::before {
                    content: "";
                    position: absolute;
                    top: 50%;
                    left: 50%;
                    width: 4px;
                    height: 4px;
                    border-radius: 100%;
                    background-color: #bdbdbd;
                }

                &::after {
                    transform: translate(-5px, -50%);
                }

                &::before {
                    transform: translate(5px, -50%);
                }
            }
        }
    }
    */

    .move {
        position: absolute;
        z-index: 1;
        top: 0;
        left: 0;
        -webkit-transform: translate(-50%, -50%) scale(0.5);
        transform: translate(-50%, -50%) scale(0.5);
        opacity: 0;
        border-radius: 100%;
        width: 40px;
        height: 40px;
        background-color: #ea5e1c;
        background: -webkit-gradient(linear, left top, right top, from(#ea5e1c), to(#ef811a));
        background: linear-gradient(to right, #ea5e1c 0%, #ef811a 100%);
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-pack: center;
        -ms-flex-pack: center;
        justify-content: center;
        -webkit-box-align: center;
        -ms-flex-align: center;
        align-items: center;
        color: white;
        font-size: 20px;
        -webkit-transition: 0.3s ease;
        transition: 0.3s ease;
        cursor: move;
        cursor: -webkit-grab;
        cursor: grab;

        span {
            -webkit-transition: 0.3s ease;
            transition: 0.3s ease;
            -webkit-transform: rotate(360deg);
            transform: rotate(360deg);
        }
    }

    .content {
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        -webkit-box-orient: vertical;
        -webkit-box-direction: normal;
        -ms-flex-direction: column;
        flex-direction: column;
        //overflow-x: hidden;

        .line {
            position: relative;
            display: -webkit-box;
            display: -ms-flexbox;
            display: flex;
            max-width: 0px;
            -webkit-box-orient: horizontal;
            -webkit-box-direction: normal;
            -ms-flex-direction: row;
            flex-direction: row;
            background-color: white;
            padding-left: 20px;
            -webkit-transition: max-width 0.3s cubic-bezier(0.015, 0.04, 0, 1);
            transition: max-width 0.3s cubic-bezier(0.015, 0.04, 0, 1);

            &.main {
                border-radius: 3px 50px 50px 3px;
            }

            &.not-main {
                opacity: 0;
                position: absolute;
                bottom: 100%;
                left: 0;
                border-radius: 3px 3px 0 0;
            }

            @for $i from 1 through 15 {
                & > :nth-child(#{$i}) {
                    -webkit-transition: all 0.3s ease, -webkit-transform 0.3s ease 0.015s * $i;
                    transition: all 0.3s ease, -webkit-transform 0.3s ease 0.015s * $i;
                    transition: all 0.3s ease, transform 0.3s ease 0.015s * $i;
                    transition: all 0.3s ease, transform 0.3s ease 0.015s * $i, -webkit-transform 0.3s ease 0.015s * $i;
                }
            }

            .item {
                display: -webkit-box;
                display: -ms-flexbox;
                display: flex;
                -webkit-box-pack: center;
                -ms-flex-pack: center;
                justify-content: center;
                -webkit-box-align: center;
                -ms-flex-align: center;
                align-items: center;
                padding: 10px;
                font-size: 24px;
                color: #5e5e5e;
                position: relative;

                min-width: 44px;
                cursor: pointer;
                -webkit-transform: scale(0);
                transform: scale(0);
                opacity: 0;
                border-radius: 3px;

                &:hover {
                    z-index: 1;
                    background-color: #e4e4e4;
                }

                &.round {
                    border-radius: 3px 50px 50px 3px;
                    padding-left: 10px;
                }

                &.red {
                    color: $ww-red;
                    &:hover {
                        background-color: $ww-red;
                        color: white;
                    }
                }
                &.green {
                    color: $ww-green;
                    &:hover {
                        background-color: $ww-green;
                        color: white;
                    }
                }
                &.orange {
                    color: $ww-orange;
                    &:hover {
                        background-color: $ww-orange;
                        color: white;
                    }
                }
                &.blue {
                    color: $ww-blue;
                    &:hover {
                        background-color: $ww-blue;
                        color: white;
                    }
                }

                &.input {
                    padding: 10px 5px;
                    input {
                        border: 1px solid $ww-blue;
                    }
                }

                &.xsmall {
                    font-size: 12px;
                }
                &.small {
                    font-size: 15px;
                }
                &.medium {
                    font-size: 18px;
                }
                &.big {
                    font-size: 21px;
                }
                &.xbig {
                    font-size: 24px;
                }

                &.font {
                    white-space: nowrap;
                    font-size: 14px !important;
                    text-align: center;
                }

                &.font-size {
                    white-space: nowrap;
                    text-align: left;
                    font-size: 14px !important;
                }

                &.font-style {
                    white-space: nowrap;
                    font-size: 14px;
                    &.white {
                        background-color: #e8e8e8;
                    }
                }

                &.colors {
                    display: -webkit-box;
                    display: -ms-flexbox;
                    display: flex;
                    -webkit-box-orient: horizontal;
                    -webkit-box-direction: normal;
                    -ms-flex-direction: row;
                    flex-direction: row;
                    padding: 15px !important;

                    // div {
                    //     border: 1px solid #a3a3a3;
                    //     width: 20px;
                    //     height: 20px;
                    //     margin: 3px;
                    //     cursor: pointer;
                    //     border-radius: 3px;
                    // }

                    &:hover {
                        background-color: transparent;
                    }

                    .color {
                        // padding: 15px 10px 15px 5px;
                        font-size: 10px;
                        width: 80px;
                        display: -webkit-box;
                        display: -ms-flexbox;
                        display: flex;
                        -webkit-box-orient: vertical;
                        -webkit-box-direction: normal;
                        -ms-flex-direction: column;
                        flex-direction: column;
                        -webkit-box-pack: center;
                        -ms-flex-pack: center;
                        justify-content: center;
                        -webkit-box-align: center;
                        -ms-flex-align: center;
                        align-items: center;

                        & > div {
                            margin-top: 10px;
                            width: 30px;
                            height: 30px;
                        }
                    }
                }

                &.expend-arrow {
                    span {
                        -webkit-transition: all 0.3s ease;
                        transition: all 0.3s ease;
                        -webkit-transform: rotate(0deg);
                        transform: rotate(0deg);
                    }
                    &.expended {
                        span {
                            -webkit-transform: rotate(-180deg);
                            transform: rotate(-180deg);
                        }
                    }
                }

                span {
                    z-index: 1;
                }

                span.fa,
                span.fas {
                    font-size: 18px;
                }

                .small-icon {
                    font-size: 18px;
                    margin-top: 4px;
                }

                input {
                    width: 42px;
                    text-align: center;
                }

                .subitems {
                    z-index: 2;
                    position: absolute;
                    pointer-events: none;
                    opacity: 0;
                    top: 100%;
                    left: 50%;
                    -webkit-transform: translate(-50%, -10px);
                    transform: translate(-50%, -10px);
                    border-radius: 3px;
                    -webkit-box-shadow: 0 2px 5px -1px rgba(99, 99, 99, 0.5);
                    box-shadow: 0 2px 5px -1px rgba(99, 99, 99, 0.5);
                    background-color: white;
                    -webkit-transition: opacity 0.2s ease,
                        -webkit-transform 0.2s
                            cubic-bezier(0.35, 0.07, 0.35, 1.65);
                    transition: opacity 0.2s ease,
                        -webkit-transform 0.2s
                            cubic-bezier(0.35, 0.07, 0.35, 1.65);
                    transition: transform 0.2s
                            cubic-bezier(0.35, 0.07, 0.35, 1.65),
                        opacity 0.2s ease;
                    transition: transform 0.2s
                            cubic-bezier(0.35, 0.07, 0.35, 1.65),
                        opacity 0.2s ease,
                        -webkit-transform 0.2s
                            cubic-bezier(0.35, 0.07, 0.35, 1.65);
                    z-index: 1;

                    &:after {
                        content: "";
                        background-color: white;
                        position: absolute;
                        top: 0;
                        left: 50%;
                        -webkit-transform: translate(-50%, -50%) rotate(45deg);
                        transform: translate(-50%, -50%) rotate(45deg);
                        width: 10px;
                        height: 10px;
                    }

                    &:before {
                        content: "";
                        width: 100%;
                        height: 10px;
                        position: absolute;
                        bottom: 100%;
                        left: 0;
                    }
                }

                &:hover .subitems {
                    pointer-events: all;
                    opacity: 1;
                    -webkit-transform: translate(-50%, 10px);
                    transform: translate(-50%, 10px);
                }
            }

            &.not-main {
                .item {
                    .subitems {
                        top: unset;
                        bottom: 100%;
                        -webkit-transform: translate(-50%, 10px);
                        transform: translate(-50%, 10px);

                        &:after {
                            top: 100%;
                        }

                        &:before {
                            top: 100%;
                            bottom: unset;
                        }
                    }

                    &:hover .subitems {
                        -webkit-transform: translate(-50%, -10px);
                        transform: translate(-50%, -10px);
                    }
                }
            }

            .item-placeholder {
                display: -webkit-box;
                display: -ms-flexbox;
                display: flex;
                width: 44px;
                position: relative;
            }

            .separator {
                width: 0;
                position: relative;
                opacity: 0;

                &::after {
                    content: "";
                    position: absolute;
                    left: 100%;
                    top: 50%;
                    -webkit-transform: translate(-50%, -50%);
                    transform: translate(-50%, -50%);
                    width: 1px;
                    height: 60%;
                    border-right: 1px solid #cbcbcb;
                }
            }
        }

        &.expended {
            .line.not-main {
                opacity: 1;
                max-width: 470px;

                .item {
                    opacity: 1;
                    -webkit-transform: scale(1);
                    transform: scale(1);
                }
                .separator {
                    opacity: 1;
                    -webkit-transform: scale(1);
                    transform: scale(1);
                }
            }
        }
    }

    &.show {
        // opacity: 1;
        // transform: translateY(-110%);

        .move {
            -webkit-transform: translate(-50%, -50%) scale(1);
            transform: translate(-50%, -50%) scale(1);
            opacity: 1;

            span {
                -webkit-transform: rotate(0deg);
                transform: rotate(0deg);
            }
        }

        .content {
            .line.main {
                max-width: 470px;

                .item {
                    opacity: 1;
                    -webkit-transform: scale(1);
                    transform: scale(1);
                }
                .separator {
                    opacity: 1;
                    -webkit-transform: scale(1);
                    transform: scale(1);
                }
            }
        }
    }
}
</style>

<style>
.no-select {
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
</style>