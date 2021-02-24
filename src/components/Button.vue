<template>
    <button type="button" :class="'w-full text-center bg-'+color+'-400 border-1 shadow border-'+color+'-700 text-gray-100 px-2 py-4 text-center text-2xl font-semibold hover:bg-'+color+'-600'">{{ label }}</button>
</template>
<script>
export default {
    props : ['label','color']
}
</script>