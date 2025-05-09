<script setup>
// ---- PrimeVue Imports -------
import { Dialog } from "primevue"

</script>

<template>

    <!-- -------- ABOUT ME DIALOG !!! ----------- -->
    <Dialog modal :header="' '" class="w-[50rem] h-[50rem] ">

        <content class="w-full flex flex-col center-all">

            <!-- Introduction Header !!! -->
            <div class="center-x w-full">

                <div class="w-[13rem] h-[13rem] bg-slate-700 center-all m-2 rounded-md cursor-pointer">
                    <p class="text-white">Too shy to take pic LOL!</p>
                </div>

                <div class="w-[25rem] ml-10 center-x flex-col">
                    <p class="text-[1.8rem] font-semibold">Jake S. <span class="text-[#77FFA4]">Dedicatoria</span> ;
                    </p>
                    <p class="text-[1rem]">
                        <span class="text-[#77FFA4]">
                            < </span>
                                Fullstack Web Developer
                                <span class="text-[#77FFA4]">
                                    />
                                </span>
                    </p>
                </div>
            </div>

            <!-- Divider !!! -->
            <div class="w-[40rem] h-[2px] flex justify-between m-5">
                <div class="w-[29rem] h-full bg bg-slate-200"></div>
                <div class="w-[10rem] h-full bg bg-[#77FFA4] "></div>
            </div>

            <!-- About me content !!! -->
            <div class="w-[40rem] h-[10rem] flex flex-col">
                <p class="mt-3 text-[1.5rem]">About <span class="text-[#77FFA4]">Me</span> ;</p>
                <p class="mt-3">Lorem ipsum dolor sit amet consectetur adipisicing elit. Dignissimos corrupti incidunt
                    earum,
                    quisquam
                    nisi maiores ipsa nam iste dolorem sed? Accusantium, rem quas laboriosam quae beatae odit accusamus
                    eveniet fuga!</p>
            </div>

            <!-- Divider !!! -->
            <div class="w-[40rem] h-[2px] flex justify-between m-5">
                <div class="w-[29rem] h-full bg bg-slate-200"></div>
                <div class="w-[10rem] h-full bg bg-[#77FFA4] "></div>
            </div>

            <!-- Key Competencies !!! -->
            <div class="w-[40rem] h-[10rem] flex flex-col">
                <div class="mt-3">
                    <p class="text-[1.5rem]">
                        Key <span class="text-[#77FFA4]">Competencies</span> ;
                    </p>
                </div>
                <div class=" mt-5 flex flex-row gap-4 flex-wrap">
                    <div v-for="i of 5" :key="i"
                        class="center-all bg-slate-500 text-slate-100 p-2 w-auto h-[2rem] rounded-md">
                        Competencies {{ i }}
                    </div>
                </div>
            </div>

        </content>

    </Dialog>

</template>