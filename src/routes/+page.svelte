<script>
  import NavbarSlider from "$lib/NavbarSlider.svelte";
  import { onMount, onDestroy } from "svelte";

  import {
    Home,
    User,
    MessageCircle,
    Settings2Icon,
    InfoIcon,
  } from "lucide-svelte";

  const menuItems = [
    { text: "Home", icon: Home },
    { text: "Profile", icon: User },
    { text: "Messages", icon: MessageCircle },
    { text: "Settings", icon: Settings2Icon },
  ];

  let windowWidth;
  let activeIndex = 0;
  let position;
  let positionMobile = "bottom";

  const MIN_MOBILE_WIDTH = 768;
  $: position = windowWidth <= MIN_MOBILE_WIDTH ? positionMobile : "left";

  function handleResize() {
    windowWidth = window.innerWidth;

    console.log("Window width: ", windowWidth);
  }

  onMount(() => {
    if (typeof window !== "undefined") {
      // Set initial window width
      windowWidth = window.innerWidth;

      // Add resize event listener
      window.addEventListener("resize", handleResize);
    }
  });

  onDestroy(() => {
    // Remove resize event listener when component is destroyed
    if (typeof window !== "undefined") {
      window.removeEventListener("resize", handleResize);
    }
  });

  function handleTabSwitch(index, item) {
    console.log(`Tab switched to ${item.text} (index: ${index})`);
    activeIndex = index;
    // Add any other logic you want to perform on tab switch
  }
</script>

<main
  class={position == "left"
    ? " flex h-screen "
    : " flex  h-screen w-full flex-col justify-center items-center" +
      (position == "top" ? " flex-col-reverse" : "")}
>
  {#if position == "left"}
    <NavbarSlider
      {menuItems}
      {position}
      {activeIndex}
      onTabSwitch={handleTabSwitch}
    />

    <div
      class={position == "left"
        ? "flex-1 flex justify-center items-center bg-[#afafb0]"
        : "flex  flex-1 justify-center items-center h-screen w-full bg-[#afafb0]"}
    >
      <h1 class="text-white text-4xl">
        {menuItems[activeIndex].text}
      </h1>
    </div>
  {:else if position == "bottom" || position == "top"}
    <div
      class="flex flex-grow bg-[#afafb0] h-screen w-full items justify-center items-center"
    >
      <h1 class="text-white text-4xl">
        {menuItems[activeIndex].text}
      </h1>
    </div>
    <NavbarSlider
      {menuItems}
      {position}
      {activeIndex}
      onTabSwitch={handleTabSwitch}
    />
  {/if}
</main>
