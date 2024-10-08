<script setup>
    import Icon from '@idc/UI2/Vue/Decor/Icon.vue';
    import { observeAttribute } from '@ux-ts/dom/Observer.ts';
    import { lang } from "@idc/Config/Config.ts";

    //
    import {reactive, watch, ref, onMounted} from "vue";

    //
    import {
        downloadImage,
        getFileList,
        selectFileEv,
        useItemEv,
        addItemEv,
        removeItemEv,
        downloadItemEv
    } from "@idc/App/Scripts/Manager.ts";

    //
    const state = reactive({
        selectedFilename: null,
        fileList: new Map([])
    })

    //
    const target = ref(null);
    const props = defineProps({
        id: { type: String, default: "#manager" },
        ifc: Boolean
    });
    onMounted(() => {
        observeAttribute(target.value, "data-hidden", ()=>{ getFileList(null, state); });
    });
    addEventListener("file-upload", ()=>getFileList(null, state));
</script>

<template>
    <div class="ui-title-label" data-transparent data-scheme="dynamic">
        <Icon inert name="wallpaper" class="ui-icon"/>
        <span>{{ "Wallpapers" }}</span>
    </div>

    <div ref="target" class="ui-screen ui-content ui-manager" :id="props.id.replace(/^\#/i,'')" v-bind="$attrs" data-scheme="solid">

        <div class="ui-nav" data-scheme="solid" style="pointer-events: auto;" data-highlight="2.5">
            <button class="menu-act hl-1 hl-2h" data-tooltip="Menu" data-scheme="solid-transparent" data-transparent data-highlight-hover="1" @click="openSidebar">
                <Icon inert slot="icon" name="menu" class="icon"/>
            </button>

            <div class="f-space"></div>

            <button data-tooltip="Use as Wallpaper" class="use-item" data-scheme="solid-transparent" data-highlight-hover="1" @click="(ev)=>useItemEv(ev, state)">
                <Icon inert name="image-play" class="icon"/>
            </button>

            <button data-tooltip="Load Image" class="add-item" data-scheme="solid-transparent" data-highlight-hover="1" @click="(ev)=>addItemEv(ev, state)">
                <Icon inert name="image-up" class="icon"/>
            </button>

            <button data-tooltip="Remove Image" class="remove-item" data-scheme="solid-transparent" data-highlight-hover="1" @click="(ev)=>removeItemEv(ev, state)">
                <Icon inert name="image-off" class="icon"/>
            </button>

            <button data-tooltip="Download Image" class="download-item" data-scheme="solid-transparent" data-highlight-hover="1" @click="(ev)=>downloadItemEv(ev, state)">
                <Icon inert name="image-down" class="icon"/>
            </button>
        </div>
        <x-scrollbox class="ui-space">

            <div class="file-list">
                <div
                    v-for="[name, file] in state.fileList"
                    @click="(ev)=>selectFileEv(ev, state)"
                    :data-filename="file.name"
                    class="file"
                    v-bind:class="{selected: state.selectedFilename == file.name}"
                    :data-scheme="state.selectedFilename == file.name ? 'inverse' : 'solid'"
                    :data-chroma="state.selectedFilename == file.name ? 0.2 : 0.0"
                    :data-alpha="state.selectedFilename == file.name ? 1.0 : 0.0"
                    data-highlight-hover="1"
                    :key="name"
                >
                    <div inert class="icon"><Icon name="wallpaper"/></div>
                    <div inert class="name">{{file.name}}</div>
                    <div inert class="date">{{new Date(file.lastModified).toLocaleString(lang)}}</div>
                </div>
            </div>

        </x-scrollbox>

    </div>
</template>

<style type="scss" lang="scss">

</style>
