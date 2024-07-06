<template>
  <div class="p-6 max-w-lg mx-auto bg-white rounded-xl shadow-md space-y-4 mb-8">
    <h1 class="text-3xl font-bold text-center text-gray-900">Tailwind CSS Class Organizer</h1>
    <div>
      <label for="input" class="block text-sm font-medium text-gray-700">Input Code</label>
      <textarea id="input" v-model="inputCode"
        class="mt-1 p-3 w-full border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        rows="5" placeholder="Paste your Tailwind CSS classes here..."></textarea>
    </div>
    <div>
      <label for="output" class="block text-sm font-medium text-gray-700">Organized Code</label>
      <textarea id="output" v-model="outputCode"
        class="mt-1 p-3 w-full border border-gray-300 rounded-md bg-gray-100 cursor-not-allowed" rows="5"
        readonly></textarea>
    </div>
    <button @click="copyToClipboard"
      class="w-full px-4 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500">
      Copy to Clipboard
    </button>
  </div>
  <div class="p-6 max-w-3xl mx-auto bg-white rounded-xl shadow-md space-y-6">
    <h2 class="text-2xl font-bold text-gray-900">How it Works</h2>
    <p class="text-gray-700 leading-relaxed">
      One of the largest problems with utility classes comes from disorganization and the inability to locate specific
      classes. Let's take, for example, a small element such as this:
    </p>
    <div class="flex justify-center items-center py-4">
      <img src="./assets/element.svg" alt="Unorganized element"
        class="rounded-md border border-gray-300 shadow-sm max-w-full">
    </div>
    <p class="text-gray-700 leading-relaxed">
      It is very difficult to locate the classes that are being used on this element. This is where this tool comes in.
      Simply paste your Tailwind CSS classes into the input box and the classes are assigned a category.
    </p>
    <div class="flex justify-center items-center py-4">
      <img src="./assets/element-labeled.svg" alt="Labeled element"
        class="rounded-md border border-gray-300 shadow-sm max-w-full">
    </div>
    <p class="text-gray-700 leading-relaxed">
      Then the elements are organized into a more legible format.
    </p>
    <div class="flex justify-center items-center py-4">
      <img src="./assets/element-sorted.svg" alt="Sorted element"
        class="rounded-md border border-gray-300 shadow-sm max-w-full">
    </div>
  </div>
</template>

<style scoped>
textarea {
  resize: none;
}
</style>


<script>
export default {
  data() {
    return {
      inputCode: "",
      outputCode: "",
      classOrderPatterns: [
        'absolute', 'block', 'box-border', 'box-content', 'clear-both', 'fixed', 'flex', 'flex-', 'flex-auto',
        'flex-basis', 'flex-col', 'flex-col-reverse', 'flex-direction', 'flex-grow', 'flex-grow-', 'flex-initial',
        'flex-none', 'flex-nowrap', 'flex-row', 'flex-row-reverse', 'flex-shrink', 'flex-shrink-', 'flex-wrap',
        'flex-wrap-reverse', 'float-left', 'float-right', 'flow-root', 'grid', 'grid-auto-columns', 'grid-auto-flow',
        'grid-auto-rows', 'grid-cols-', 'grid-cols-none', 'grid-cols-subgrid', 'grid-column', 'grid-column-end',
        'grid-column-start', 'grid-flow-col', 'grid-flow-col-dense', 'grid-flow-dense', 'grid-flow-row',
        'grid-flow-row-dense', 'grid-row', 'grid-row-end', 'grid-row-start', 'grid-rows-', 'grid-rows-none',
        'grid-rows-subgrid', 'grid-template-columns', 'grid-template-rows', 'inline', 'inline-block', 'inline-flex',
        'inline-grid', 'inline-table', 'inset-auto', 'm-', 'm-auto', 'm-px', 'mb-', 'mb-auto', 'mb-px', 'ml-', 'ml-auto',
        'ml-px', 'mr-', 'mr-auto', 'mr-px', 'mt-', 'mt-auto', 'mt-px', 'mx-', 'mx-auto', 'mx-px', 'my-', 'my-auto',
        'my-px', 'object-contain', 'object-cover', 'object-fill', 'object-none', 'object-scale-down', 'overflow-auto',
        'overflow-hidden', 'overflow-scroll', 'overflow-visible', 'overflow-x-auto', 'overflow-x-hidden', 'overflow-x-scroll',
        'overflow-x-visible', 'overflow-y-auto', 'overflow-y-hidden', 'overflow-y-scroll', 'overflow-y-visible', 'p-', 'p-px',
        'pb-', 'pb-px', 'pl-', 'pl-px', 'pr-', 'pr-px', 'pt-', 'pt-px', 'px-', 'px-px', 'py-', 'py-px', 'relative', 'static',
        'sticky', 'table', 'table-auto', 'table-caption', 'table-cell', 'table-column', 'table-column-group', 'table-fixed',
        'table-footer-group', 'table-header-group', 'table-layout', 'table-row', 'table-row-group', 'z-auto', 'auto-cols-auto',
        'auto-cols-fr', 'auto-cols-max', 'auto-cols-min', 'auto-rows-auto', 'auto-rows-fr', 'auto-rows-max', 'auto-rows-min',
        'basis-', 'basis-auto', 'basis-full', 'basis-px', 'content-around', 'content-between', 'content-center', 'content-end',
        'content-evenly', 'content-start', 'gap-', 'gap-px', 'gap-x-', 'gap-x-px', 'gap-y-', 'gap-y-px', 'grow', 'grow-',
        'items-baseline', 'items-center', 'items-end', 'items-start', 'items-stretch', 'justify-around', 'justify-between',
        'justify-center', 'justify-end', 'justify-evenly', 'justify-start', 'order-first', 'order-last', 'order-none',
        'self-auto', 'self-center', 'self-end', 'self-start', 'self-stretch', 'shrink', 'shrink-', 'space-x-', 'space-x-px',
        'space-x-reverse', 'space-y-', 'space-y-px', 'space-y-reverse', 'h-', 'h-auto', 'h-dvh', 'h-fit', 'h-full', 'h-lvh',
        'h-max', 'h-min', 'h-px', 'h-screen', 'h-svh', 'max-h-', 'max-h-dvh', 'max-h-fit', 'max-h-full', 'max-h-lvh', 'max-h-max',
        'max-h-min', 'max-h-none', 'max-h-px', 'max-h-screen', 'max-h-svh', 'max-w-', 'max-w-fit', 'max-w-full', 'max-w-lg',
        'max-w-max', 'max-w-md', 'max-w-min', 'max-w-none', 'max-w-prose', 'max-w-px', 'max-w-screen-', 'max-w-screen-lg',
        'max-w-screen-md', 'max-w-screen-sm', 'max-w-screen-xl', 'max-w-sm', 'max-w-xl', 'max-w-xs', 'min-h-', 'min-h-dvh',
        'min-h-fit', 'min-h-full', 'min-h-lvh', 'min-h-max', 'min-h-min', 'min-h-px', 'min-h-screen', 'min-h-svh', 'min-w-',
        'min-w-fit', 'min-w-full', 'min-w-max', 'min-w-min', 'min-w-px', 'w-', 'w-auto', 'w-dvw', 'w-fit', 'w-full', 'w-lvw',
        'w-max', 'w-min', 'w-px', 'w-screen', 'w-svw', 'antialiased', 'capitalize', 'font-black', 'font-bold', 'font-extrabold',
        'font-extralight', 'font-light', 'font-medium', 'font-mono', 'font-normal', 'font-sans', 'font-semibold', 'font-serif',
        'font-thin', 'leading-loose', 'leading-none', 'leading-normal', 'leading-relaxed', 'leading-snug', 'leading-tight',
        'line-through', 'lowercase', 'no-underline', 'normal-case', 'subpixel-antialiased', 'text-base', 'text-black', 'text-blue-',
        'text-center', 'text-current', 'text-gray-', 'text-green-', 'text-indigo-', 'text-justify', 'text-left', 'text-lg',
        'text-opacity-', 'text-pink-', 'text-purple-', 'text-red-', 'text-right', 'text-sm', 'text-transparent', 'text-white',
        'text-xl', 'text-xs', 'text-yellow-', 'tracking-normal', 'tracking-tight', 'tracking-tighter', 'tracking-wide',
        'tracking-wider', 'tracking-widest', 'underline', 'underline-offset-', 'underline-offset-auto', 'uppercase', 'bg-auto',
        'bg-black', 'bg-blue-', 'bg-bottom', 'bg-center', 'bg-contain', 'bg-cover', 'bg-current', 'bg-fixed', 'bg-gray-',
        'bg-green-', 'bg-indigo-', 'bg-left', 'bg-left-bottom', 'bg-left-top', 'bg-local', 'bg-no-repeat', 'bg-opacity-',
        'bg-pink-', 'bg-purple-', 'bg-red-', 'bg-repeat', 'bg-repeat-round', 'bg-repeat-space', 'bg-repeat-x', 'bg-repeat-y',
        'bg-right', 'bg-right-bottom', 'bg-right-top', 'bg-scroll', 'bg-top', 'bg-transparent', 'bg-white', 'bg-yellow-', 'border', 'border-',
        'border-amber-', 'border-b', 'border-b-', 'border-b-amber-', 'border-b-black', 'border-b-blue-', 'border-b-current',
        'border-b-cyan-', 'border-b-emerald-', 'border-b-fuchsia-', 'border-b-gray-', 'border-b-green-', 'border-b-indigo-',
        'border-b-inherit', 'border-b-lime-', 'border-b-neutral-', 'border-b-orange-', 'border-b-pink-', 'border-b-purple-',
        'border-b-red-', 'border-b-rose-', 'border-b-sky-', 'border-b-slate-', 'border-b-stone-', 'border-b-teal-',
        'border-b-transparent', 'border-b-violet-', 'border-b-white', 'border-b-yellow-', 'border-b-zinc-', 'border-black',
        'border-blue-', 'border-bottom-color', 'border-bottom-left-radius', 'border-bottom-right-radius', 'border-bottom-width',
        'border-collapse', 'border-color', 'border-current', 'border-cyan-', 'border-dashed', 'border-dotted', 'border-double',
        'border-e', 'border-e-', 'border-e-amber-', 'border-e-black', 'border-e-blue-', 'border-e-current', 'border-e-cyan-',
        'border-e-emerald-', 'border-e-fuchsia-', 'border-e-gray-', 'border-e-green-', 'border-e-indigo-', 'border-e-inherit',
        'border-e-lime-', 'border-e-neutral-', 'border-e-orange-', 'border-e-pink-', 'border-e-purple-', 'border-e-red-',
        'border-e-rose-', 'border-e-sky-', 'border-e-slate-', 'border-e-stone-', 'border-e-teal-', 'border-e-transparent',
        'border-e-violet-', 'border-e-white', 'border-e-yellow-', 'border-e-zinc-', 'border-emerald-', 'border-end-end-radius',
        'border-end-start-radius', 'border-fuchsia-', 'border-gray-', 'border-green-', 'border-hidden', 'border-indigo-',
        'border-inherit', 'border-inline-end-color', 'border-inline-end-width', 'border-inline-start-color',
        'border-inline-start-width', 'border-l', 'border-l-', 'border-l-amber-', 'border-l-black', 'border-l-blue-',
        'border-l-current', 'border-l-cyan-', 'border-l-emerald-', 'border-l-fuchsia-', 'border-l-gray-', 'border-l-green-',
        'border-l-indigo-', 'border-l-inherit', 'border-l-lime-', 'border-l-neutral-', 'border-l-orange-', 'border-l-pink-',
        'border-l-purple-', 'border-l-red-', 'border-l-rose-', 'border-l-sky-', 'border-l-slate-', 'border-l-stone-',
        'border-l-teal-', 'border-l-transparent', 'border-l-violet-', 'border-l-white', 'border-l-yellow-', 'border-l-zinc-',
        'border-left-color', 'border-left-width', 'border-lime-', 'border-neutral-', 'border-none', 'border-opacity-',
        'border-orange-', 'border-pink-', 'border-purple-', 'border-r', 'border-r-', 'border-r-amber-', 'border-r-black',
        'border-r-blue-', 'border-r-current', 'border-r-cyan-', 'border-r-emerald-', 'border-r-fuchsia-', 'border-r-gray-',
        'border-r-green-', 'border-r-indigo-', 'border-r-inherit', 'border-r-lime-', 'border-r-neutral-', 'border-r-orange-',
        'border-r-pink-', 'border-r-purple-', 'border-r-red-', 'border-r-rose-', 'border-r-sky-', 'border-r-slate-',
        'border-r-stone-', 'border-r-teal-', 'border-r-transparent', 'border-r-violet-', 'border-r-white', 'border-r-yellow-',
        'border-r-zinc-', 'border-radius', 'border-red-', 'border-right-color', 'border-right-width', 'border-rose-', 'border-s',
        'border-s-', 'border-s-amber-', 'border-s-black', 'border-s-blue-', 'border-s-current', 'border-s-cyan-',
        'border-s-emerald-', 'border-s-fuchsia-', 'border-s-gray-', 'border-s-green-', 'border-s-indigo-', 'border-s-inherit',
        'border-s-lime-', 'border-s-neutral-', 'border-s-orange-', 'border-s-pink-', 'border-s-purple-', 'border-s-red-',
        'border-s-rose-', 'border-s-sky-', 'border-s-slate-', 'border-s-stone-', 'border-s-teal-', 'border-s-transparent',
        'border-s-violet-', 'border-s-white', 'border-s-yellow-', 'border-s-zinc-', 'border-separate', 'border-sky-',
        'border-slate-', 'border-solid', 'border-spacing', 'border-spacing-', 'border-spacing-px', 'border-spacing-x-',
        'border-spacing-x-px', 'border-spacing-y-', 'border-spacing-y-px', 'border-start-end-radius', 'border-start-start-radius',
        'border-stone-', 'border-style', 'border-t', 'border-t-', 'border-t-amber-', 'border-t-black', 'border-t-blue-',
        'border-t-current', 'border-t-cyan-', 'border-t-emerald-', 'border-t-fuchsia-', 'border-t-gray-', 'border-t-green-',
        'border-t-indigo-', 'border-t-inherit', 'border-t-lime-', 'border-t-neutral-', 'border-t-orange-', 'border-t-pink-',
        'border-t-purple-', 'border-t-red-', 'border-t-rose-', 'border-t-sky-', 'border-t-slate-', 'border-t-stone-',
        'border-t-teal-', 'border-t-transparent', 'border-t-violet-', 'border-t-white', 'border-t-yellow-', 'border-t-zinc-',
        'border-teal-', 'border-top-color', 'border-top-left-radius', 'border-top-right-radius', 'border-top-width',
        'border-transparent', 'border-violet-', 'border-white', 'border-width', 'border-x', 'border-x-', 'border-x-amber-',
        'border-x-black', 'border-x-blue-', 'border-x-current', 'border-x-cyan-', 'border-x-emerald-', 'border-x-fuchsia-',
        'border-x-gray-', 'border-x-green-', 'border-x-indigo-', 'border-x-inherit', 'border-x-lime-', 'border-x-neutral-',
        'border-x-orange-', 'border-x-pink-', 'border-x-purple-', 'border-x-red-', 'border-x-rose-', 'border-x-sky-',
        'border-x-slate-', 'border-x-stone-', 'border-x-teal-', 'border-x-transparent', 'border-x-violet-', 'border-x-white',
        'border-x-yellow-', 'border-x-zinc-', 'border-y', 'border-y-', 'border-y-amber-', 'border-y-black', 'border-y-blue-',
        'border-y-current', 'border-y-cyan-', 'border-y-emerald-', 'border-y-fuchsia-', 'border-y-gray-', 'border-y-green-',
        'border-y-indigo-', 'border-y-inherit', 'border-y-lime-', 'border-y-neutral-', 'border-y-orange-', 'border-y-pink-',
        'border-y-purple-', 'border-y-red-', 'border-y-rose-', 'border-y-sky-', 'border-y-slate-', 'border-y-stone-',
        'border-y-teal-', 'border-y-transparent', 'border-y-violet-', 'border-y-white', 'border-y-yellow-', 'border-y-zinc-',
        'border-yellow-', 'border-zinc-', 'rounded-', 'rounded-b', 'rounded-b-', 'rounded-b-full', 'rounded-b-lg', 'rounded-b-md',
        'rounded-b-none', 'rounded-b-sm', 'rounded-b-xl', 'rounded-bl', 'rounded-bl-', 'rounded-bl-full', 'rounded-bl-lg',
        'rounded-bl-md', 'rounded-bl-none', 'rounded-bl-sm', 'rounded-bl-xl', 'rounded-br', 'rounded-br-', 'rounded-br-full',
        'rounded-br-lg', 'rounded-br-md', 'rounded-br-none', 'rounded-br-sm', 'rounded-br-xl', 'rounded-e', 'rounded-e-',
        'rounded-e-full', 'rounded-e-lg', 'rounded-e-md', 'rounded-e-none', 'rounded-e-sm', 'rounded-e-xl', 'rounded-ee',
        'rounded-ee-', 'rounded-ee-full', 'rounded-ee-lg', 'rounded-ee-md', 'rounded-ee-none', 'rounded-ee-sm', 'rounded-ee-xl',
        'rounded-es', 'rounded-es-', 'rounded-es-full', 'rounded-es-lg', 'rounded-es-md', 'rounded-es-none', 'rounded-es-sm',
        'rounded-es-xl', 'rounded-full', 'rounded-l', 'rounded-l-', 'rounded-l-full', 'rounded-l-lg', 'rounded-l-md',
        'rounded-l-none', 'rounded-l-sm', 'rounded-l-xl', 'rounded-lg', 'rounded-md', 'rounded-none', 'rounded-r', 'rounded-r-',
        'rounded-r-full', 'rounded-r-lg', 'rounded-r-md', 'rounded-r-none', 'rounded-r-sm', 'rounded-r-xl', 'rounded-s',
        'rounded-s-', 'rounded-s-full', 'rounded-s-lg', 'rounded-s-md', 'rounded-s-none', 'rounded-s-sm', 'rounded-s-xl',
        'rounded-se', 'rounded-se-', 'rounded-se-full', 'rounded-se-lg', 'rounded-se-md', 'rounded-se-none', 'rounded-se-sm',
        'rounded-se-xl', 'rounded-sm', 'rounded-ss', 'rounded-ss-', 'rounded-ss-full', 'rounded-ss-lg', 'rounded-ss-md',
        'rounded-ss-none', 'rounded-ss-sm', 'rounded-ss-xl', 'rounded-t', 'rounded-t-', 'rounded-t-full', 'rounded-t-lg',
        'rounded-t-md', 'rounded-t-none', 'rounded-t-sm', 'rounded-t-xl', 'rounded-tl', 'rounded-tl-', 'rounded-tl-full',
        'rounded-tl-lg', 'rounded-tl-md', 'rounded-tl-none', 'rounded-tl-sm', 'rounded-tl-xl', 'rounded-tr', 'rounded-tr-',
        'rounded-tr-full', 'rounded-tr-lg', 'rounded-tr-md', 'rounded-tr-none', 'rounded-tr-sm', 'rounded-tr-xl', 'rounded-xl',
        'backdrop-blur-', 'backdrop-blur-lg', 'backdrop-blur-md', 'backdrop-blur-none', 'backdrop-blur-sm', 'backdrop-blur-xl',
        'backdrop-brightness-', 'backdrop-contrast-', 'backdrop-filter', 'backdrop-filter-none', 'backdrop-grayscale-',
        'backdrop-hue-rotate-', 'backdrop-invert-', 'backdrop-opacity-', 'backdrop-saturate-', 'backdrop-sepia-', 'blur-',
        'blur-lg', 'blur-md', 'blur-none', 'blur-sm', 'blur-xl', 'brightness-', 'contrast-', 'filter', 'filter-none',
        'grayscale-', 'hue-rotate-', 'invert-', 'mix-blend-color', 'mix-blend-color-burn', 'mix-blend-color-dodge',
        'mix-blend-darken', 'mix-blend-difference', 'mix-blend-exclusion', 'mix-blend-hard-light', 'mix-blend-hue',
        'mix-blend-lighten', 'mix-blend-luminosity', 'mix-blend-multiply', 'mix-blend-normal', 'mix-blend-overlay',
        'mix-blend-saturation', 'mix-blend-screen', 'mix-blend-soft-light', 'opacity-', 'saturate-', 'sepia-', 'shadow',
        'shadow-', 'shadow-amber-', 'shadow-black', 'shadow-blue-', 'shadow-current', 'shadow-cyan-', 'shadow-emerald-',
        'shadow-fuchsia-', 'shadow-gray-', 'shadow-green-', 'shadow-indigo-', 'shadow-inherit', 'shadow-inner', 'shadow-lg',
        'shadow-lime-', 'shadow-md', 'shadow-neutral-', 'shadow-none', 'shadow-orange-', 'shadow-pink-', 'shadow-purple-',
        'shadow-red-', 'shadow-rose-', 'shadow-sky-', 'shadow-slate-', 'shadow-sm', 'shadow-stone-', 'shadow-teal-',
        'shadow-transparent', 'shadow-violet-', 'shadow-white', 'shadow-xl', 'shadow-yellow-', 'shadow-zinc-', 'cursor-auto',
        'cursor-default', 'cursor-move', 'cursor-not-allowed', 'cursor-pointer', 'cursor-text', 'cursor-wait',
        'pointer-events-auto', 'pointer-events-none', 'resize', 'resize-none', 'resize-x', 'resize-y', 'select-all',
        'select-auto', 'select-none', 'select-text', 'fill-current', 'stroke-', 'stroke-amber-', 'stroke-black', 'stroke-blue-',
        'stroke-current', 'stroke-cyan-', 'stroke-emerald-', 'stroke-fuchsia-', 'stroke-gray-', 'stroke-green-', 'stroke-indigo-',
        'stroke-inherit', 'stroke-lime-', 'stroke-neutral-', 'stroke-none', 'stroke-orange-', 'stroke-pink-', 'stroke-purple-',
        'stroke-red-', 'stroke-rose-', 'stroke-sky-', 'stroke-slate-', 'stroke-stone-', 'stroke-teal-', 'stroke-transparent',
        'stroke-violet-', 'stroke-white', 'stroke-width', 'stroke-yellow-', 'stroke-zinc-', 'not-sr-only', 'sr-only',
        'appearance-none', 'isolate', 'isolation-auto', '-backdrop-hue-rotate-', '-bottom-', '-bottom-full', '-bottom-px',
        '-col-end-', '-col-start-', '-end-', '-end-full', '-end-px', '-hue-rotate-', '-indent-', '-indent-px', '-inset-',
        '-inset-full', '-inset-px', '-inset-x-', '-inset-x-full', '-inset-x-px', '-inset-y-', '-inset-y-full', '-inset-y-px',
        '-left-', '-left-full', '-left-px', '-m-', '-m-px', '-mb-', '-mb-px', '-me-', '-me-px', '-ml-', '-ml-px', '-moz-appearance',
        '-moz-column-break-after', '-moz-column-break-before', '-moz-column-break-inside', '-moz-column-gap', '-moz-columns',
        '-moz-osx-font-smoothing', '-moz-user-select', '-mr-', '-mr-px', '-ms-', '-ms-px', '-mt-', '-mt-px', '-mx-', '-mx-px',
        '-my-', '-my-px', '-o-object-fit', '-o-object-position', '-order-', '-order-first', '-order-last', '-order-none',
        '-outline-offset-', '-right-', '-right-full', '-right-px', '-rotate-', '-row-end-', '-row-start-', '-scale-', '-scale-x-',
        '-scale-y-', '-scroll-m-', '-scroll-m-px', '-scroll-mb-', '-scroll-mb-px', '-scroll-me-', '-scroll-me-px', '-scroll-ml-',
        '-scroll-ml-px', '-scroll-mr-', '-scroll-mr-px', '-scroll-ms-', '-scroll-ms-px', '-scroll-mt-', '-scroll-mt-px',
        '-scroll-mx-', '-scroll-mx-px', '-scroll-my-', '-scroll-my-px', '-skew-x-', '-skew-y-', '-space-x-', '-space-x-px',
        '-space-y-', '-space-y-px', '-start-', '-start-full', '-start-px', '-top-', '-top-full', '-top-px', '-tracking-normal',
        '-tracking-tight', '-tracking-tighter', '-tracking-wide', '-tracking-wider', '-tracking-widest', '-translate-x-',
        '-translate-x-full', '-translate-x-px', '-translate-y-', '-translate-y-full', '-translate-y-px', '-webkit-appearance',
        '-webkit-backdrop-filter', '-webkit-background-clip', '-webkit-box-decoration-break', '-webkit-box-orient',
        '-webkit-font-smoothing', '-webkit-hyphens', '-webkit-line-clamp', '-webkit-user-select', '-z-', 'accent-amber-',
        'accent-auto', 'accent-black', 'accent-blue-', 'accent-color', 'accent-current', 'accent-cyan-', 'accent-emerald-',
        'accent-fuchsia-', 'accent-gray-', 'accent-green-', 'accent-indigo-', 'accent-inherit', 'accent-lime-', 'accent-neutral-',
        'accent-orange-', 'accent-pink-', 'accent-purple-', 'accent-red-', 'accent-rose-', 'accent-sky-', 'accent-slate-',
        'accent-stone-', 'accent-teal-', 'accent-transparent', 'accent-violet-', 'accent-white', 'accent-yellow-', 'accent-zinc-',
        'align-baseline', 'align-bottom', 'align-content', 'align-items', 'align-middle', 'align-self', 'align-sub', 'align-super',
        'align-text-bottom', 'align-text-top', 'align-top', 'animate-bounce', 'animate-none', 'animate-ping', 'animate-pulse',
        'animate-spin', 'animation', 'animation-timing-function', 'appearance', 'appearance-auto', 'aspect-auto', 'aspect-ratio',
        'aspect-square', 'aspect-video', 'backdrop-blur', 'backdrop-grayscale', 'backdrop-invert', 'backdrop-sepia',
        'background-attachment', 'background-blend-mode', 'background-clip', 'background-color', 'background-image',
        'background-origin', 'background-position', 'background-repeat', 'background-size', 'bg-amber-', 'bg-blend-color',
        'bg-blend-color-burn', 'bg-blend-color-dodge', 'bg-blend-darken', 'bg-blend-difference', 'bg-blend-exclusion',
        'bg-blend-hard-light', 'bg-blend-hue', 'bg-blend-lighten', 'bg-blend-luminosity', 'bg-blend-multiply', 'bg-blend-normal',
        'bg-blend-overlay', 'bg-blend-saturation', 'bg-blend-screen', 'bg-blend-soft-light', 'bg-clip-border', 'bg-clip-content',
        'bg-clip-padding', 'bg-clip-text', 'bg-cyan-', 'bg-emerald-', 'bg-fuchsia-', 'bg-gradient-to-b', 'bg-gradient-to-bl',
        'bg-gradient-to-br', 'bg-gradient-to-l', 'bg-gradient-to-r', 'bg-gradient-to-t', 'bg-gradient-to-tl', 'bg-gradient-to-tr',
        'bg-inherit', 'bg-lime-', 'bg-neutral-', 'bg-none', 'bg-orange-', 'bg-origin-border', 'bg-origin-content', 'bg-origin-padding',
        'bg-rose-', 'bg-sky-', 'bg-slate-', 'bg-stone-', 'bg-teal-', 'bg-violet-', 'bg-zinc-', 'blur', 'border', 'bottom', 'bottom-',
        'bottom-auto', 'bottom-full', 'bottom-px', 'box-decoration-break', 'box-decoration-clone', 'box-decoration-slice', 'box-shadow',
        'box-sizing', 'break-after', 'break-after-all', 'break-after-auto', 'break-after-avoid', 'break-after-avoid-page',
        'break-after-column', 'break-after-left', 'break-after-page', 'break-after-right', 'break-all', 'break-before',
        'break-before-all', 'break-before-auto', 'break-before-avoid', 'break-before-avoid-page', 'break-before-column',
        'break-before-left', 'break-before-page', 'break-before-right', 'break-inside', 'break-inside-auto', 'break-inside-avoid',
        'break-inside-avoid-column', 'break-inside-avoid-page', 'break-keep', 'break-normal', 'break-words', 'caption-bottom',
        'caption-side', 'caption-top', 'caret-amber-', 'caret-black', 'caret-blue-', 'caret-color', 'caret-current', 'caret-cyan-',
        'caret-emerald-', 'caret-fuchsia-', 'caret-gray-', 'caret-green-', 'caret-indigo-', 'caret-inherit', 'caret-lime-',
        'caret-neutral-', 'caret-orange-', 'caret-pink-', 'caret-purple-', 'caret-red-', 'caret-rose-', 'caret-sky-', 'caret-slate-',
        'caret-stone-', 'caret-teal-', 'caret-transparent', 'caret-violet-', 'caret-white', 'caret-yellow-', 'caret-zinc-', 'clear',
        'clear-end', 'clear-left', 'clear-none', 'clear-right', 'clear-start', 'clip', 'col-auto', 'col-end-', 'col-end-auto',
        'col-span-', 'col-span-full', 'col-start-', 'col-start-auto', 'collapse', 'color', 'column-gap', 'columns', 'columns-',
        'columns-auto', 'columns-lg', 'columns-md', 'columns-sm', 'columns-xl', 'columns-xs', 'contain', 'contain-content',
        'contain-inline-size', 'contain-layout', 'contain-none', 'contain-paint', 'contain-size', 'contain-strict', 'contain-style',
        'content', 'content-baseline', 'content-none', 'content-normal', 'content-stretch', 'contents', 'cursor', 'cursor-alias',
        'cursor-all-scroll', 'cursor-cell', 'cursor-col-resize', 'cursor-context-menu', 'cursor-copy', 'cursor-crosshair',
        'cursor-e-resize', 'cursor-ew-resize', 'cursor-grab', 'cursor-grabbing', 'cursor-help', 'cursor-n-resize', 'cursor-ne-resize',
        'cursor-nesw-resize', 'cursor-no-drop', 'cursor-none', 'cursor-ns-resize', 'cursor-nw-resize', 'cursor-nwse-resize',
        'cursor-progress', 'cursor-row-resize', 'cursor-s-resize', 'cursor-se-resize', 'cursor-sw-resize', 'cursor-vertical-text',
        'cursor-w-resize', 'cursor-zoom-in', 'cursor-zoom-out', 'decoration-', 'decoration-amber-', 'decoration-auto',
        'decoration-black', 'decoration-blue-', 'decoration-clone', 'decoration-current', 'decoration-cyan-', 'decoration-dashed',
        'decoration-dotted', 'decoration-double', 'decoration-emerald-', 'decoration-from-font', 'decoration-fuchsia-',
        'decoration-gray-', 'decoration-green-', 'decoration-indigo-', 'decoration-inherit', 'decoration-lime-', 'decoration-neutral-',
        'decoration-orange-', 'decoration-pink-', 'decoration-purple-', 'decoration-red-', 'decoration-rose-', 'decoration-sky-',
        'decoration-slate-', 'decoration-slice', 'decoration-solid', 'decoration-stone-', 'decoration-teal-', 'decoration-transparent',
        'decoration-violet-', 'decoration-wavy', 'decoration-white', 'decoration-yellow-', 'decoration-zinc-', 'delay-',
        'diagonal-fractions', 'display', 'divide-amber-', 'divide-black', 'divide-blue-', 'divide-current', 'divide-cyan-',
        'divide-dashed', 'divide-dotted', 'divide-double', 'divide-emerald-', 'divide-fuchsia-', 'divide-gray-', 'divide-green-',
        'divide-indigo-', 'divide-inherit', 'divide-lime-', 'divide-neutral-', 'divide-none', 'divide-opacity-', 'divide-orange-',
        'divide-pink-', 'divide-purple-', 'divide-red-', 'divide-rose-', 'divide-sky-', 'divide-slate-', 'divide-solid',
        'divide-stone-', 'divide-teal-', 'divide-transparent', 'divide-violet-', 'divide-white', 'divide-x', 'divide-x-',
        'divide-x-reverse', 'divide-y', 'divide-y-', 'divide-y-reverse', 'divide-yellow-', 'divide-zinc-', 'drop-shadow',
        'drop-shadow-', 'drop-shadow-lg', 'drop-shadow-md', 'drop-shadow-none', 'drop-shadow-sm', 'drop-shadow-xl', 'duration-',
        'ease-in', 'ease-in-out', 'ease-linear', 'ease-out', 'end-', 'end-auto', 'end-full', 'end-px', 'fill', 'fill-amber-',
        'fill-black', 'fill-blue-', 'fill-cyan-', 'fill-emerald-', 'fill-fuchsia-', 'fill-gray-', 'fill-green-', 'fill-indigo-',
        'fill-inherit', 'fill-lime-', 'fill-neutral-', 'fill-none', 'fill-orange-', 'fill-pink-', 'fill-purple-', 'fill-red-',
        'fill-rose-', 'fill-sky-', 'fill-slate-', 'fill-stone-', 'fill-teal-', 'fill-transparent', 'fill-violet-', 'fill-white',
        'fill-yellow-', 'fill-zinc-', 'float', 'float-end', 'float-none', 'float-start', 'font-family', 'font-size', 'font-style',
        'font-variant-numeric', 'font-weight', 'forced-color-adjust', 'forced-color-adjust-auto', 'forced-color-adjust-none',
        'from-', 'from-amber-', 'from-black', 'from-blue-', 'from-current', 'from-cyan-', 'from-emerald-', 'from-fuchsia-',
        'from-gray-', 'from-green-', 'from-indigo-', 'from-inherit', 'from-lime-', 'from-neutral-', 'from-orange-', 'from-pink-',
        'from-purple-', 'from-red-', 'from-rose-', 'from-sky-', 'from-slate-', 'from-stone-', 'from-teal-', 'from-transparent',
        'from-violet-', 'from-white', 'from-yellow-', 'from-zinc-', 'gap', 'grayscale', 'height', 'hidden', 'hyphens',
        'hyphens-auto', 'hyphens-manual', 'hyphens-none', 'indent-', 'indent-px', 'inset', 'inset-', 'inset-full', 'inset-inline-end',
        'inset-inline-start', 'inset-px', 'inset-x-', 'inset-x-auto', 'inset-x-full', 'inset-x-px', 'inset-y-', 'inset-y-auto',
        'inset-y-full', 'inset-y-px', 'invert', 'invisible', 'isolation', 'italic', 'justify-content', 'justify-items',
        'justify-items-center', 'justify-items-end', 'justify-items-start', 'justify-items-stretch', 'justify-normal',
        'justify-self', 'justify-self-auto', 'justify-self-center', 'justify-self-end', 'justify-self-start', 'justify-self-stretch',
        'justify-stretch', 'leading-', 'left', 'left-', 'left-auto', 'left-full', 'left-px', 'letter-spacing', 'line-clamp-',
        'line-clamp-none', 'line-height', 'lining-nums', 'list-decimal', 'list-disc', 'list-image-none', 'list-inside', 'list-item',
        'list-none', 'list-outside', 'list-style-image', 'list-style-position', 'list-style-type', 'margin', 'margin-bottom',
        'margin-inline-end', 'margin-inline-start', 'margin-left', 'margin-right', 'margin-top', 'max-height', 'max-width', 'me-',
        'me-auto', 'me-px', 'min-height', 'min-width', 'mix-blend-mode', 'mix-blend-plus-darker', 'mix-blend-plus-lighter', 'ms-',
        'ms-auto', 'ms-px', 'normal-nums', 'not-italic', 'object-bottom', 'object-center', 'object-fit', 'object-left',
        'object-left-bottom', 'object-left-top', 'object-position', 'object-right', 'object-right-bottom', 'object-right-top',
        'object-top', 'oldstyle-nums', 'opacity', 'order', 'order-', 'ordinal', 'origin-bottom', 'origin-bottom-left',
        'origin-bottom-right', 'origin-center', 'origin-left', 'origin-right', 'origin-top', 'origin-top-left', 'origin-top-right',
        'outline', 'outline-', 'outline-amber-', 'outline-black', 'outline-blue-', 'outline-color', 'outline-current',
        'outline-cyan-', 'outline-dashed', 'outline-dotted', 'outline-double', 'outline-emerald-', 'outline-fuchsia-',
        'outline-gray-', 'outline-green-', 'outline-indigo-', 'outline-inherit', 'outline-lime-', 'outline-neutral-',
        'outline-none', 'outline-offset', 'outline-offset-', 'outline-orange-', 'outline-pink-', 'outline-purple-', 'outline-red-',
        'outline-rose-', 'outline-sky-', 'outline-slate-', 'outline-stone-', 'outline-style', 'outline-teal-', 'outline-transparent',
        'outline-violet-', 'outline-white', 'outline-width', 'outline-yellow-', 'outline-zinc-', 'overflow', 'overflow-clip',
        'overflow-ellipsis', 'overflow-wrap', 'overflow-x', 'overflow-x-clip', 'overflow-y', 'overflow-y-clip', 'overline',
        'overscroll-auto', 'overscroll-behavior', 'overscroll-behavior-x', 'overscroll-behavior-y', 'overscroll-contain', 'overscroll-none',
        'overscroll-x-auto', 'overscroll-x-contain', 'overscroll-x-none', 'overscroll-y-auto', 'overscroll-y-contain', 'overscroll-y-none',
        'padding', 'padding-bottom', 'padding-inline-end', 'padding-inline-start', 'padding-left', 'padding-right', 'padding-top', 'pe-',
        'pe-px', 'place-content', 'place-content-around', 'place-content-baseline', 'place-content-between', 'place-content-center',
        'place-content-end', 'place-content-evenly', 'place-content-start', 'place-content-stretch', 'place-items',
        'place-items-baseline', 'place-items-center', 'place-items-end', 'place-items-start', 'place-items-stretch', 'place-self',
        'place-self-auto', 'place-self-center', 'place-self-end', 'place-self-start', 'place-self-stretch', 'placeholder-amber-',
        'placeholder-black', 'placeholder-blue-', 'placeholder-current', 'placeholder-cyan-', 'placeholder-emerald-',
        'placeholder-fuchsia-', 'placeholder-gray-', 'placeholder-green-', 'placeholder-indigo-', 'placeholder-inherit',
        'placeholder-lime-', 'placeholder-neutral-', 'placeholder-opacity-', 'placeholder-orange-', 'placeholder-pink-',
        'placeholder-purple-', 'placeholder-red-', 'placeholder-rose-', 'placeholder-sky-', 'placeholder-slate-',
        'placeholder-stone-', 'placeholder-teal-', 'placeholder-transparent', 'placeholder-violet-', 'placeholder-white',
        'placeholder-yellow-', 'placeholder-zinc-', 'pointer-events', 'position', 'proportional-nums', 'ps-', 'ps-px', 'right',
        'right-', 'right-auto', 'right-full', 'right-px', 'ring', 'ring-', 'ring-amber-', 'ring-black', 'ring-blue-', 'ring-current',
        'ring-cyan-', 'ring-emerald-', 'ring-fuchsia-', 'ring-gray-', 'ring-green-', 'ring-indigo-', 'ring-inherit', 'ring-inset',
        'ring-lime-', 'ring-neutral-', 'ring-offset-', 'ring-offset-amber-', 'ring-offset-black', 'ring-offset-blue-',
        'ring-offset-current', 'ring-offset-cyan-', 'ring-offset-emerald-', 'ring-offset-fuchsia-', 'ring-offset-gray-',
        'ring-offset-green-', 'ring-offset-indigo-', 'ring-offset-inherit', 'ring-offset-lime-', 'ring-offset-neutral-',
        'ring-offset-orange-', 'ring-offset-pink-', 'ring-offset-purple-', 'ring-offset-red-', 'ring-offset-rose-',
        'ring-offset-sky-', 'ring-offset-slate-', 'ring-offset-stone-', 'ring-offset-teal-', 'ring-offset-transparent',
        'ring-offset-violet-', 'ring-offset-white', 'ring-offset-yellow-', 'ring-offset-zinc-', 'ring-opacity-', 'ring-orange-',
        'ring-pink-', 'ring-purple-', 'ring-red-', 'ring-rose-', 'ring-sky-', 'ring-slate-', 'ring-stone-', 'ring-teal-',
        'ring-transparent', 'ring-violet-', 'ring-white', 'ring-yellow-', 'ring-zinc-', 'rotate-', 'rounded', 'row-auto',
        'row-end-', 'row-end-auto', 'row-gap', 'row-span-', 'row-span-full', 'row-start-', 'row-start-auto', 'scale-', 'scale-x-',
        'scale-y-', 'scroll-auto', 'scroll-behavior', 'scroll-m-', 'scroll-m-px', 'scroll-margin', 'scroll-margin-bottom',
        'scroll-margin-inline-end', 'scroll-margin-inline-start', 'scroll-margin-left', 'scroll-margin-right', 'scroll-margin-top',
        'scroll-mb-', 'scroll-mb-px', 'scroll-me-', 'scroll-me-px', 'scroll-ml-', 'scroll-ml-px', 'scroll-mr-', 'scroll-mr-px',
        'scroll-ms-', 'scroll-ms-px', 'scroll-mt-', 'scroll-mt-px', 'scroll-mx-', 'scroll-mx-px', 'scroll-my-', 'scroll-my-px',
        'scroll-p-', 'scroll-p-px', 'scroll-padding', 'scroll-padding-bottom', 'scroll-padding-inline-end',
        'scroll-padding-inline-start', 'scroll-padding-left', 'scroll-padding-right', 'scroll-padding-top', 'scroll-pb-',
        'scroll-pb-px', 'scroll-pe-', 'scroll-pe-px', 'scroll-pl-', 'scroll-pl-px', 'scroll-pr-', 'scroll-pr-px', 'scroll-ps-',
        'scroll-ps-px', 'scroll-pt-', 'scroll-pt-px', 'scroll-px-', 'scroll-px-px', 'scroll-py-', 'scroll-py-px', 'scroll-smooth',
        'scroll-snap-align', 'scroll-snap-stop', 'scroll-snap-type', 'self-baseline', 'sepia', 'size-', 'size-auto', 'size-fit',
        'size-full', 'size-max', 'size-min', 'size-px', 'skew-x-', 'skew-y-', 'slashed-zero', 'snap-align-none', 'snap-always',
        'snap-both', 'snap-center', 'snap-end', 'snap-mandatory', 'snap-none', 'snap-normal', 'snap-proximity', 'snap-start',
        'snap-x', 'snap-y', 'stacked-fractions', 'start-', 'start-auto', 'start-full', 'start-px', 'stroke', 'tabular-nums',
        'text-', 'text-align', 'text-amber-', 'text-balance', 'text-clip', 'text-cyan-', 'text-decoration-color',
        'text-decoration-line', 'text-decoration-style', 'text-decoration-thickness', 'text-ellipsis', 'text-emerald-',
        'text-end', 'text-fuchsia-', 'text-indent', 'text-inherit', 'text-lime-', 'text-neutral-', 'text-nowrap', 'text-orange-',
        'text-overflow', 'text-pretty', 'text-rose-', 'text-sky-', 'text-slate-', 'text-start', 'text-stone-', 'text-teal-',
        'text-transform', 'text-underline-offset', 'text-violet-', 'text-wrap', 'text-zinc-', 'to', 'to-', 'to-amber-', 'to-black',
        'to-blue-', 'to-current', 'to-cyan-', 'to-emerald-', 'to-fuchsia-', 'to-gray-', 'to-green-', 'to-indigo-', 'to-inherit',
        'to-lime-', 'to-neutral-', 'to-orange-', 'to-pink-', 'to-purple-', 'to-red-', 'to-rose-', 'to-sky-', 'to-slate-', 'to-stone-',
        'to-teal-', 'to-transparent', 'to-violet-', 'to-white', 'to-yellow-', 'to-zinc-', 'top', 'top-', 'top-auto', 'top-full',
        'top-px', 'touch-action', 'touch-auto', 'touch-manipulation', 'touch-none', 'touch-pan-down', 'touch-pan-left',
        'touch-pan-right', 'touch-pan-up', 'touch-pan-x', 'touch-pan-y', 'touch-pinch-zoom', 'transform', 'transform-cpu',
        'transform-gpu', 'transform-none', 'transform-origin', 'transition', 'transition-all', 'transition-colors', 'transition-delay',
        'transition-duration', 'transition-none', 'transition-opacity', 'transition-property', 'transition-shadow',
        'transition-timing-function', 'transition-transform', 'translate-x-', 'translate-x-full', 'translate-x-px', 'translate-y-',
        'translate-y-full', 'translate-y-px', 'truncate', 'tw-backdrop-blur', 'tw-backdrop-brightness', 'tw-backdrop-contrast',
        'tw-backdrop-grayscale', 'tw-backdrop-hue-rotate', 'tw-backdrop-invert', 'tw-backdrop-opacity', 'tw-backdrop-saturate',
        'tw-backdrop-sepia', 'tw-bg-opacity', 'tw-blur', 'tw-border-opacity', 'tw-border-spacing-x', 'tw-border-spacing-y',
        'tw-brightness', 'tw-contain-layout', 'tw-contain-paint', 'tw-contain-size', 'tw-contain-style', 'tw-content', 'tw-contrast',
        'tw-divide-opacity', 'tw-divide-x-reverse', 'tw-divide-y-reverse', 'tw-drop-shadow', 'tw-gradient-from',
        'tw-gradient-from-position', 'tw-gradient-stops', 'tw-gradient-to', 'tw-gradient-to-position', 'tw-gradient-via-position',
        'tw-grayscale', 'tw-hue-rotate', 'tw-invert', 'tw-numeric-figure', 'tw-numeric-fraction', 'tw-numeric-spacing', 'tw-ordinal',
        'tw-pan-x', 'tw-pan-y', 'tw-pinch-zoom', 'tw-placeholder-opacity', 'tw-ring-color', 'tw-ring-inset', 'tw-ring-offset-color',
        'tw-ring-offset-shadow', 'tw-ring-offset-width', 'tw-ring-opacity', 'tw-ring-shadow', 'tw-rotate', 'tw-saturate', 'tw-scale-x',
        'tw-scale-y', 'tw-scroll-snap-strictness', 'tw-sepia', 'tw-shadow', 'tw-shadow-color', 'tw-shadow-colored', 'tw-skew-x', 'tw-skew-y',
        'tw-slashed-zero', 'tw-space-x-reverse', 'tw-space-y-reverse', 'tw-text-opacity', 'tw-translate-x', 'tw-translate-y',
        'user-select', 'vertical-align', 'via-', 'via-amber-', 'via-black', 'via-blue-', 'via-current', 'via-cyan-', 'via-emerald-',
        'via-fuchsia-', 'via-gray-', 'via-green-', 'via-indigo-', 'via-inherit', 'via-lime-', 'via-neutral-', 'via-orange-',
        'via-pink-', 'via-purple-', 'via-red-', 'via-rose-', 'via-sky-', 'via-slate-', 'via-stone-', 'via-teal-', 'via-transparent',
        'via-violet-', 'via-white', 'via-yellow-', 'via-zinc-', 'visibility', 'visible', 'white-space', 'whitespace-break-spaces',
        'whitespace-normal', 'whitespace-nowrap', 'whitespace-pre', 'whitespace-pre-line', 'whitespace-pre-wrap', 'width',
        'will-change', 'will-change-auto', 'will-change-contents', 'will-change-scroll', 'will-change-transform', 'word-break', 'z-', 'z-index',
      ],
      classCategories: ['Layout & Positioning', 'Flexbox and Grid', 'Spacing', 'Sizing', 'Typography', 'Backgrounds', 'Borders', 'Effects', 'Transitions & Animations', 'Interactivity', 'SVG', 'Accessibility', 'Miscellaneous'],
    };
  },
  watch: {
    inputCode(newVal) {
      this.outputCode = this.reorderClasses(newVal);
    },
  },
  methods: {
    reorderClasses(input) {
      const classPattern = /class="([^"]*)"/g;
      let match;
      let result = input;

      while ((match = classPattern.exec(input)) !== null) {
        const classes = match[1].split(" ").filter(cls => cls.trim());

        classes.sort((a, b) => {
          const indexA = this.classOrderPatterns.findIndex(pattern => a.includes(pattern));
          const indexB = this.classOrderPatterns.findIndex(pattern => b.includes(pattern));

          if (indexA === -1 && indexB === -1) return 0;
          if (indexA === -1) return 1;
          if (indexB === -1) return -1;

          return indexA - indexB;
        });

        const orderedClasses = classes.join(" ");
        result = result.replace(match[0], `class="${orderedClasses}"`);
      }

      return result;
    },
    copyToClipboard() {
      navigator.clipboard.writeText(this.outputCode).then(() => {
      });
    },
  },
};
</script>

<style scoped>
/* Additional styles can be added here if needed */
</style>
