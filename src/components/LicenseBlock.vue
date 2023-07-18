<template>
  <div class="absolute bottom-20 border-t pt-2 w-full">
    <div class="flex justify-right space-x-5 pr-5  pt-2"> 
      <span >License Key</span> <input  type="text" v-model="licenseKey" placeholder="Unlicensed" class="w-1/2 rounded-[7px] border border-blue-gray-200  bg-transparent px-3 font-sans text-sm font-normal text-blue-gray-700 outline outline-0 transition-all placeholder-shown:border placeholder-shown:border-blue-gray-200 placeholder-shown:border-t-blue-gray-200 focus:border-2 focus:outline-0 disabled:bg-blue-gray-50" />
    </div>
    <div class="flex justify-right space-x-5 pr-5 pt-2"> 
      <span >Api Key</span> <input  type="text" v-model="apiKey" placeholder="Enter API Key" class="w-1/2 rounded-[7px] border border-blue-gray-200  bg-transparent px-3 font-sans text-sm font-normal text-blue-gray-700 outline outline-0 transition-all placeholder-shown:border placeholder-shown:border-blue-gray-200 placeholder-shown:border-t-blue-gray-200 focus:border-2 focus:outline-0 disabled:bg-blue-gray-50" />
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'LicenseBlock',
  data() {
    return {
      licenseKey: '',
      apiKey: ''
    }
  }
}

</script>