<template>
    <my-page title="MIT 协议" :page="page">
        <ui-article class="article">
            <h1>MIT License</h1>
            <p></p>
            <p>Copyright (c)
                <ui-text-field v-model="time" v-if="isConfig" />
                <span v-else>{{ time }}</span>

                <ui-text-field v-model="owner" v-if="isConfig" />
                <span v-else>{{ owner }}</span>
            </p>
            <p>Permission is hereby granted, free of charge, to any person obtaining a copy
                of this software and associated documentation files (the "Software"), to deal
                in the Software without restriction, including without limitation the rights
                to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
                copies of the Software, and to permit persons to whom the Software is
                furnished to do so, subject to the following conditions:</p>
            <p></p>
            <p>The above copyright notice and this permission notice shall be included in all
                copies or substantial portions of the Software.</p>
            <p></p>
            <p>THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
                IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
                FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
                AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
                LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
                OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
                SOFTWARE.</p>
        </ui-article>
        <ul>
            <li></li>
        </ul>
        <div class="btns">
            <ui-raised-button class="btn" label="下载" ref="button" @click="toggle"/>
            <ui-raised-button class="btn" label="配置" @click="config"/>
            <ui-popover :trigger="trigger" :open="open" @close="handleClose">
                <ui-menu>
                    <ui-menu-item title="LICENSE.md" @click="download('LICENSE.md')" />
                    <ui-menu-item title="LICENSE.txt" @click="download('LICENSE.txt')" />
                    <!--<ui-menu-item title="LICENSE" @click="download('LICENSE')" />-->
                </ui-menu>
            </ui-popover>
        </div>
    </my-page>
</template>

<script>
    const saveAs = window.saveAs

    export default {
        data () {
            return {
                isConfig: false,
                time: '',
                owner: '',
                open: false,
                trigger: null,
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'check',
                            click: this.finish,
                            visible: false
                        }
                    ]
                }
            }
        },
        mounted() {
            this.owner = this.$route.query.owner || 'yunser.com'
            this.time = this.$route.query.time || '2016-2018'

            this.trigger = this.$refs.button.$el
        },
        methods: {
            toggle () {
                this.open = !this.open
            },
            handleClose (e) {
                this.open = false
            },
            config() {
                this.isConfig = !this.isConfig
            },
            finish() {
                this.isConfig = false
            },
            download(filename) {
                let content = `MIT License

Copyright (c) ${this.time} ${this.owner}

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.`
                let blob = new Blob([content], {type: 'text/plain;charset=utf-8'})
                saveAs(blob, filename)
            }
        }
    }
</script>

<style lang="scss" scoped>
    .article {
        max-width: 750px;
    }

    .btns {
        margin-top: 40px;
        .btn {
            margin-right: 8px;
        }
    }
</style>
