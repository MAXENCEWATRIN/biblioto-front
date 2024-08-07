<template>
    <div class="mr-10pl max-w-lg mx-auto p-4 grid grid-cols-2 gap-4">
        <div @drop.prevent="handleDrop" @dragover.prevent @dragenter.prevent
            class="flex items-center justify-center border-2 border-dashed text-white border-gray-400 rounded-lg p-6 cursor-pointer"
            @click="selectFile">
            <input type="file" ref="fileInput" class="hidden" @change="handleFileChange" accept=".jpg" />
            <p>Drag & Drop your JPG file here or click to browse</p>
        </div>
        <div  v-if="editImage && !previewUrl" class="mt-4 grid grid-cols-2 gap-4">
            <img :src="editImage" class="max-w-xs max-h-xs object-contain rounded-lg" />
        </div>
        <div v-if="previewUrl" class="mt-4 grid grid-cols-2 gap-4">
            <img :src="previewUrl" alt="Preview" class="max-w-xs max-h-xs object-contain rounded-lg" />
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
    name: 'ImageUploader',
    props: {
        editImage: {
            type: String as PropType<String>,
            required: false
        }
    },
    setup(props) {
        const fileInput = ref<HTMLInputElement | null>(null);
        const previewUrl = ref<string | null>(null);

        const handleDrop = (event: DragEvent) => {
            const files = event.dataTransfer?.files;
            if (files && files.length > 0) {
                handleFile(files[0]);
            }
        };

        const handleFileChange = (event: Event) => {
            const input = event.target as HTMLInputElement;
            if (input.files && input.files.length > 0) {
                handleFile(input.files[0]);
            }
        };

        const handleFile = (file: File) => {
            if (file.type === 'image/jpeg') {
                const reader = new FileReader();
                reader.onload = (e) => {
                    previewUrl.value = e.target?.result as string;
                    
                };
                reader.readAsDataURL(file);
            } else {
                alert('Only JPG files are accepted.');
            }
        };

        const selectFile = () => {
            fileInput.value?.click();
        };

        return {
            fileInput,
            previewUrl,
            handleDrop,
            handleFileChange,
            selectFile,
        };
    },
});
</script>

<style scoped>
.cursor-pointer {
    cursor: pointer;
}
</style>