<template>
    <li>
        <div @click='toggle'>
            <!--<i v-if='isFolder' class="fa " :class="[open?'fa-folder-open':'fa-folder']"></i>
            isFolder判断是否存在子级改变图标
            <i v-if='!isFolder' class="fa fa-file-text"></i> -->{{model.Title}}
        </div>
        <ul v-show="open" v-if='isFolder'>
            <itemsTree v-for='cel in model.arr' :model='cel'></itemsTree>
        </ul>
    </li>
</template>

<script>
    export default {
        name: 'itemsTree',
        props: ['model'],
        components: {},
        data() {
            return {
                open: false,
                isFolder: true
            }
        },
        computed: {
            isFolder: function() {
                return this.model.arr && this.model.arr.length
            }
        },
        methods: {
            toggle: function() {
                if(this.isFolder) {
                    this.open = !this.open
                }
            },
        }
    }
</script>