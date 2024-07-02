<script>
  import { onMount } from "svelte";

  export let position = "left"; // whether the menubar should be at the bottom or top
  export let menuItems = []; // the menu items and their icons
  export let onTabSwitch = (index, item) => {}; // the callback function to be called when a tab is switched
  export let activeIndex = 0; // the active index of current menu item

  let highlightColor = "bg-[#2196f3]"; // the color of the active link, it cannot be exported then doesnt show up in the parent component

  function setActiveLink(index) {
    if (activeIndex !== index) {
      activeIndex = index;
      // Call the callback prop
      onTabSwitch(index, menuItems[index]);
    }
  }

  onMount(() => {});
</script>

<div
  class={position === "left"
    ? "navigation bg-white h-full min-h-screen rounded-r-[15px]  min-w-[100px] flex flex-col items-start justify-start  z-10"
    : "navigation bg-white w-full min-w-[300px] h-[70px] rounded-[15px] grid  place-items-center z-10"}
>
  <ul
    class={position == "left"
      ? "list-none w-full relative"
      : "list-none flex relative"}
  >
    {#each menuItems as item, index}
      <li
        class="py-[5px] px-[2px] z-[3] cursor-pointer w-[75px]"
        class:active={activeIndex === index}
        on:click={() => setActiveLink(index)}
      >
        <a
          href="#"
          class="no-underline text-[#222327] flex flex-col items-center"
        >
          <span
            class="text text-[1.05em] font-medium text-blue-400 py-[5px] px-[5px] rounded-[12px] opacity-0 transform -translate-y-[30px] transition-all duration-500 ease-in-out
                  {activeIndex === index
              ? ' translate-y-[10px] opacity-100'
              : ''}"
          >
            {item.text}
          </span>
          <span
            class="icon text-[1.5em] transform -translate-y-[20px] transition-all duration-500 ease-in-out
                  {activeIndex === index
              ? position == 'top'
                ? 'translate-y-[26px] text-white'
                : 'translate-y-[-64px] text-white'
              : ''}"
          >
            <svelte:component this={item.icon} size={24} />
          </span>
        </a>
      </li>
    {/each}
    {#if position == "left"}
      <div
        class="indicator w-[50px] h-[50px] rounded-full border-[#222327] {highlightColor} absolute right-[-38px] z-[2] transition-all duration-300 ease-in-out flex items-center justify-center"
        style="top: {activeIndex * 72 + 2}px;"
      >
        <span class="icon text-white">
          <svelte:component this={menuItems[activeIndex].icon} size={24} />
        </span>
      </div>
    {:else}
      <div
        class="indicator w-[50px] h-[50px] rounded-full border-[#222327] {highlightColor} absolute {position ==
        'top'
          ? '-bottom-[48%]'
          : '-top-[54%]'}  z-[2] transition-all duration-300 ease-in-out
       "
            style="left: {activeIndex < 2 ? activeIndex * 75 + 12: activeIndex *75 + 12 }px;"
      ></div>
    {/if}
  </ul>
</div>
