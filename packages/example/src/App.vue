<template>
    <Nav />
    <main>
        <div class="max-w-5xl mx-auto py-6 sm:px-6 lg:px-8">
            <div class="px-4 py-6 sm:px-0">
                <div class="border-4 border-gray-200 h-64 rounded-lg">
                    <VPerfectSignature
                        :stroke-options="strokeOptions"
                        ref="signaturePad"
                    />
                </div>
                <div class="mt-10 space-x-4 text-center">
                    <button @click="clear" class="clear-btn">
                        Clear and draw again
                    </button>
                    <button @click="download" class="download-btn">
                        Download signature
                    </button>
                </div>
            </div>
        </div>
    </main>
</template>

<script setup lang="ts">
import VPerfectSignature from 'v-perfect-signature'
import Nav from './components/Nav.vue'
import { ref } from 'vue'

const strokeOptions = {
    size: 8,
    thinning: 0.5,
    smoothing: 0.5,
    streamline: 0.5,
    last: true,
}

const signaturePad = ref<typeof VPerfectSignature>()

const clear = () => {
    signaturePad.value?.clear()
}

const download = () => {
    if (signaturePad.value?.isEmpty()) {
        alert('Empty signature pad!')
        return
    }

    const link = document.createElement('a')
    link.download = 'signature.png'
    link.href = signaturePad.value?.toDataURL()
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
}
</script>

<style>
.clear-btn {
    @apply px-4 py-2 font-medium tracking-wide text-gray-800 hover:text-white  capitalize transition-colors duration-200 transform rounded-md bg-white border border-gray-800 hover:bg-gray-700 focus:outline-none focus:text-white  focus:bg-gray-700;
}

.download-btn {
    @apply px-4 py-2 font-medium tracking-wide text-white hover:text-gray-800 capitalize transition-colors duration-200 transform rounded-md bg-gray-800 hover:bg-white hover:border-gray-800 hover:border  focus:outline-none;
}
</style>