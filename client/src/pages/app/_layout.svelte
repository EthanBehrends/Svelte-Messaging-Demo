<script>
import Sidebar from "@components/Sidebar.svelte";
import ChatContainer from "@components/ChatContainer.svelte";
import user from "@core/user"
import { goto } from "@roxi/routify"
import { onMount } from "svelte"
import Icon from "@iconify/svelte"

$: if (!$user.loggedIn) $goto("/auth/login")

let expanded = false
let windowWidth

$: isSmall = windowWidth < 640

$: if (!isSmall) expanded = false
</script>

<svelte:window bind:innerWidth={windowWidth} />


<div class="w-full h-full flex flex-col">
    <div class="z-100 h-15 flex items-center justify-between p-4 header shadow">
        <div class="flex items-center gap-4">
            {#if isSmall}
            <div class="cursor-pointer" on:click={() => expanded = true}>
                <Icon icon="charm:menu-hamburger" height="1.5rem" />
            </div>
            {/if}
            <div class="font-semibold text-2xl">SvelteCord</div>
        </div>
        <div class="text-sm cursor-pointer hover:underline">Hello, {$user.firstName}</div>
    </div>

    <div class="flex-grow flex overflow-hidden">
        <div class="z-90 w-0 sm:w-60 flex-shrink-0 shadow relative bg-cool-gray-300">
            <div class="sidebar-container absolute z-80 w-60 top-0 left-0 bottom-0" on:channel_selected={() => expanded = false} class:isSmall class:expanded>
                <Sidebar />
            </div>
        </div>
        <div class="h-full w-full relative">
            {#if expanded && isSmall}
            <div class="bg-cool-gray-700 opacity-25 absolute inset-0 z-50" on:click={() => expanded = false}></div>
            {/if}
            <slot />
        </div>
    </div>
</div>

<style>
    .header {
        background: linear-gradient(180deg,hsl(196deg 41% 13%) 61%, hsl(196deg 11% 19%));
        color: white;
    }

    .sidebar-container {
        position: absolute;
        top: 0px;
        bottom: 0px;
        left: 0px;
        transition: .2s;
    }

    .sidebar-container.isSmall {
        left: -15rem;
    }

    .sidebar-container.expanded {
        left: 0px;
    }
</style>